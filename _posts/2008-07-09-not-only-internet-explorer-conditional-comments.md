---
title: "(not only) Internet Explorer conditional comments"
---

<p><em>Internet Explorer</em> conditional comments are probably the best way to hack IE bugs. The way simple and easy in use.Syntax is following :</p>

<pre>
&lt;!--[if expression]> 
HTML
&lt;![endif]-->
</pre>

<p>Thanks to mere HTML comments code is hidden from all browsers, except <em>Internet Explorer</em>, which parses it if expression returns <code>true</code>. Expression is pretty flexible. You can detect <em>Internet Explorer</em> in general (<code>[if IE]</code>) as well as its particular versions (<code>[if IE 7]</code>). You can even use comparision (<code>[if lte IE 7]</code>) and logical (<code>[if !(IE 6)]</code>, <code>[if (IE 6)|(IE gte 8)]</code>) operators. You can find some examples at the bottom.</p>

<p>You should by no means bother about <em>Internet Explorer</em> 5.x and lower as <a href="http://en.wikipedia.org/wiki/Internet_Explorer#Market_adoption">its usage is hardly noticeable</a>. Probably just some dinosaurs somewhere and victims of fossilized corporate rules (feel sorry for you, really).</p>

<p>Simple like that.</p>

<p>But, <strong>what if you want to hide code from <em>Internet Explorer</em> and to display it in other browsers?</strong></p>

<p>At first glance you might say it's impossible using syntax above. We can't order other browsers to parse and display code inside HTML comments, can we? Right, we can't, but what we can do is to trick all browsers using following syntax:</p>

<pre>
&lt;!--[if !IE]> -->
HTML
&lt;!-- &lt;![endif]-->
</pre>

<p>It may look a little messy (actually if you ever had to use or write any dirty IE hacks, it should not), but it works. Table below illustrates what browsers parsers see and do:</p>

<table>
	<thead>
		<tr>
			<th>Code</th>
			<th>Internet Explorer parser</th>
			<th>Other browsers parsers</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><code>&lt;!--[if&nbsp;!IE]&gt;</code></td>
			<td>IE conditional comment opening tag<br/>(expression says not to parse the code in HTML comment)</td>
			<td>HTML comment opening tag</td>
		</tr>
		<tr>
			<td><code>--&gt;</code></td>
			<td>ignores it</td>
			<td>HTML comment closing tag</td>
		</tr>
		<tr>
			<td>HTML</td>
			<td>ignores it</td>
			<td>parses HTML code and displays it</td>
		</tr>
		<tr>
			<td><code>&lt;!--</code></td>
			<td>ignores it</td>
			<td>HTML comment opening tag</td>
		</tr>
		<tr>
			<td><code>&lt;![endif]--&gt;</code></td>
			<td>IE conditional comment closing tag</td>
			<td>HTML comment closing tag</td>
		</tr>
	</tbody>
</table>

<p>So it's exactly what we wanted, Internet Explorer ignores the code and other browsers parses it. Pretty nice, isn't it?</p>

<p>In closing, here are some ready-to-copy-and-use examples of conditional statements with short explanation:</p>

<ul>
<li><code>&lt;!--[if IE 8]&gt; HTML &lt;![endif]--&gt;</code> - if Internet Explorer 8</li>
<li><code>&lt;!--[if !(IE 8)]&gt; HTML &lt;![endif]--&gt;</code> - if not Internet Explorer 8</li>
<li><code>&lt;!--[if lt IE 8]&gt; HTML &lt;![endif]--&gt;</code> - if older than Internet Explorer 8 (lt = lower than)</li>
<li><code>&lt;!--[if gt IE 7]&gt; HTML &lt;![endif]--&gt;</code> - if newer than Internet Explorer 7 (gt = grater than)</li>
<li><code>&lt;!--[if lte IE 7]&gt; HTML &lt;![endif]--&gt;</code> - if Internet Explorer 7 or lower (lte = lower than or equal)</li>
<li><code>&lt;!--[if (IE 7)|(IE 8)]&gt; HTML &lt;![endif]--&gt;</code> - if Internet Explorer 7 or 8</li>
<li><code>&lt;!--[if (gte IE 6)&(lte IE 8)]&gt; HTML &lt;![endif]--&gt;</code> -  if Internet Explorer between 6 and 8 (inclusive)</li>
<li><code>&lt;!--[if !IE]&gt; --&gt; HTML &lt;!-- &lt;![endif]--&gt; </code> - if any browser, except Internet Explorer</li>
<li><code>&lt;!--[if IE]&gt; html, body { display: none } &lt;![endif]--&gt;</code> - the best IE hack ever, it solves all bugs, no other hacks needed anymore<sup>*</sup></li>
</ul>

<p><small><sup>*</sup> - it's  just joke of course. I saw it on some blog, don't remember author right now, sorry.</small></p>
