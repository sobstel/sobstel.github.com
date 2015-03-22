---
title: "Converting hex to base-36 number with high precision (128 bit)"
---

In-built MySQL `CONV()` function works with 64-bit precision only. It makes it useless if you want to convert UUID or MD5 hash to base-36 number for example.

The only solution I've figured out is to code own custom function, which converts from base-16 to base-10, and then from base-10 to base-36.

<pre>
DROP FUNCTION IF EXISTS conv_hex_to_base36;

DELIMITER $$
CREATE FUNCTION conv_hex_to_base36(_hex TEXT)
    RETURNS TEXT
BEGIN
    DECLARE _hex_len TINYINT;
    DECLARE _dec DECIMAL(65);
    DECLARE _chars CHAR(36);
    DECLARE _base36 TEXT;
    DECLARE _mod TINYINT;

    SET _hex_len = LENGTH(_hex);

    IF (_hex_len > 32) THEN
        SIGNAL SQLSTATE '40001' \
        SET MESSAGE_TEXT = \
        'Hex is out of range (max 32 chars)';
    END IF;

    -- convert from base-16 to base-10
    SET _dec = CAST(
        CONV(
            SUBSTRING(_hex, -LEAST(16, _hex_len)
        ), 16, 10)
        AS DECIMAL(65)
    );
    IF _hex_len > 16 THEN
        SET _dec = _dec + CAST(
            CONV(
                SUBSTRING(_hex, 1, _hex_len - 16), 16, 10)
            AS DECIMAL(65)
            ) * 18446744073709551616;
    END IF;

    -- convert from base-10 to base-36
    SET _chars = "0123456789abcdefghijklmnopqrstuvwxyz";
    SET _base36 = "";
    WHILE _dec > 0 DO
        SET _mod = _dec % 36;
        SET _base36 = CONCAT(
            SUBSTRING(_chars, _mod + 1, 1),
            _base36
        );
        SET _dec = FLOOR(_dec / 36);
    END WHILE;

    return _base36;
END;
$$
DELIMITER ;
</pre>

If you know any better (and smarter) way, please let me know!

A few additional notes:

- It does not work for bigger (than 128-bit) numbers.
- `DECIMAL(65)` must be used (and not `DOUBLE PRECISION` for example), otherwise it generates false results without any notice.
- Functions are defined per database. If you want it to access globally [you can create shared database](http://dba.stackexchange.com/questions/50678/mysql-possibility-to-create-global-routines-stored-procedures-and-or-functions) and then call it like this `shared.conv_hex_to_base36`.
- If you want to test above func, do not use PHP's `base_convert`. It returns false values for big numbers.
- Unlike Ruby, which works well with big numbers (`ruby -e 'print "ffff".to_i(16).to_s(36)'`).



