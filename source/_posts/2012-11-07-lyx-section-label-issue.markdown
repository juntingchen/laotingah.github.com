---
layout: post
title: "LyX section label issue"
date: 2012-11-07 22:32
comments: true
categories: 
---

It had been a headache to me for over 2 days dealing with the section label problem in LyX. It ended up with a bug report sent to both LyX developer forum and the author of spconf.sty, an IEEE paper formatting package for LaTeX. The turn out is the LyX developers have found an alternative solution years ago and they insist to "*educating the user by making it impossible to put a label in a section*". Therefore, I find it necessary to broadcast this to the potential LyX rookies.

The story is the following. If you put a label in the environment of a section just following the section name (sounds quite logical, isn't it?), LyX generates the LaTeX source as follows,

LyX source:

{% img /images/2012-11-07-lyx-section-label-issue/section_label_1.png %}

LaTeX source output:
{% codeblock %}
\subsection{Two Timescale User Scheduling \label{sub:Two-Timescale-scheduling}}
{% endcodeblock %}

Note that the \label{} is placed within the section macro. This is fine in general, but it may generate problems for some package, e.g. the spconf.sty package. Using spconf.sty, you may encounter this warning

*LaTeX Warning: Reference 'sub:Two-Timescale-scheduling' underfined*

It is said that it generate spurious latex errors when using "non-latin 1 codepages" [1].

The LyX forum suggest to label the section in this way,

LyX Source:

{% img /images/2012-11-07-lyx-section-label-issue/section_label_2.png %}

Then the LaTeX source will be:
{% codeblock %}
\subsection{Two Timescale User Scheduling}

\label{sub:Two-Timescale-scheduling}

Conventional throughput optimal (in stability ...
{% endcodeblock %}

which follows the LaTeX code convention. 

So the conclusion is, so avoid various mysterious trouble, just change your LyX code habit!


See also:
[1] The LyX project ticket track [#2154].
[#2154]:http://www.lyx.org/trac/ticket/2154