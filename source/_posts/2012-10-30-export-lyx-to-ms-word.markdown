---
layout: post
title: "Export LyX to MS Word"
date: 2012-10-30 20:55
comments: true
categories: 
---
Normally, we seldom put a LyX document into Word, since the reason we use LyX is for better handling the math equations and the layout, which MS Word is truly bad at. However, you might probably run into a situation one day that your boss requests only a Word document. Then you need the exporting. As you may know, the equations would be your nightmare.

I have just exported a long LyX document into two 70-page MS Word documents. One is a survey report, mixed with text, pictures and equations, and, another is a technical report, pages of mathematics equations. I actually spent 10+ hours for the whole process, including a couple of hours searching for a one-off exporting method (of course, it does not exist!) and a painful night for fine-tuning the exported Word document. 

I will show my final way to get it through, and compare with my other attempts.

## General approaches
There is a comprehensive overview for the exporting LaTeX to other text-processor available in [TUG]. Here is a summary of my attempts, in particular useful as reference for LyX users.

1. **Directly type the text and equation to Word**
	- Pros: keeping the beautiful and editable math equations, and best for short documents
	- Cons: It would be a disaster for long documents.
	- Tips: You can just copy and paste the body of LyX document to Word. The math equations will expand into LaTeX format, which you can convert by [MathType] or [Aurora]. [MathType] now can support direct simple LaTeX input. [Aurora] is a Word add-in that supports more complex LaTeX semantics, but it is commercial. 
	
	Note: It is worthy to note the “Paste from TeX” function of the Aurora add-in. You can copy a paragraph from TeX source to the clipboard, and then migrate the content to Word using this function. The equations are beautifully converted to Aurora objects. However, the cross-reference, citations and numbering are not handled.

2. **Use a Word import filter**
	- Pros: exporting long documents at instance, keeping most structural/font formatting styles, fully editable math equations, and on-off solution for documents with only simple equations
	- Cons: usually commercial, may not support complex LaTeX equations, and usually cannot translate all your complex symbols
	
	I tried [TeX2Word]. It costs USD99, but you can always download a trial version (only translate 3 pages) first and buy after you have tested everything fine. Note that capability for transforming math equations is dominated by the Word add-in [MathType]. For example, [TeX2Word 2.0] even cannot recognize $\mathcal{}$ and results in an fatal error. For the [TeX2Word 3.0] version, it leaves a blank for the $\mathcal{}$, and you need to check line by line for you equations.

3.	**LyX -> LaTeX -> RTF -> Word**

	I tried [latex2rtf]. It simply stopped at some “unknown control sequence”. I think the results of this solution are just similar as using a Word importer.
	
4. **LyX -> HTML -> Word (*RECOMMENDED*)**
	- Pros: suitable for documents with many complex math equations
	- Cons: need a lot post-fine-tuning
	
	It is an intermediate trade-off between methods 1 and 2, and works universally for all kinds of LyX documents. The exporter converts all displayable characters into text (e.g., normal style symbols, superscripts/subscripts) and others into images. 

## Key flows in LyX -> HTML -> Word

1.	Open your document in LyX. Export the file using File->Export->HTML. The program will then create a subfolder containing all the images at the same location as your source file.

2.	Open the largest html file in the subfolder in Word. Select all and copy.

3.	Create a new document in Word and Paste. Note, this is to keep your document structure, e.g., the title and the levels of headings/sections.

	Note: [1] There are a few other options in exporting, e.g., LyxHTML and HTML (MS Word). However, these options simply did not work for me.
[2] The exported cannot support image file type other than EPS (may depend on your LyX and LaTeX package versions). I used [JPG-PNGTOEPS] to batch-convert all my images.

4. Find tuning in Word

	+ *Insert all the footnotes*

	Footnotes are exported in separated html files in sequence. You need to copy them back into your main Word file.

	+ *Fine tune the images and tables*

	Resize the image / substitute the low resolution exported EPS-jpeg images into clearer ones. Give them captions if you may need.

	+ *Fine tune the equations*

	The inline layout for equation images may be ugly and need adjusting.

	+ *Break the linked image in Word file (IMPORTANT)*

	By default, the images are not stored in the doc file but linked to the exported subfolder. The way to break them is, 
	- In Word 2010, first convert the file into .docx format. 
	- Then go to File->Info, at the rightmost, click into “Edit link to Files”. 
	- Inside, select all the graphic files, check the box “Save picture in document” and click “Break Link”. 
	- Then you are done.

## Open issues
Although the LyX -> HTML -> Word method seems to best fit into my situation, it still requires a lot of efforts for fine tuning. What’s more, I spotted some issues and hope someone can help me improve. 

1.	Only EPS image format is supported in LyX->HTML exporting (LyX 2.0.0)

2.	Low resolution of the converted images

3.	Incorrect mixed superscript/subscript text for simple math equations

	For example, $U_M^2$ would becomes $U_M\,^^2$. Things get worse with superscript’s superscript, etc. I think the reason is LyX thinks HTML can display such superscripts/subscripts and so that refuse to convert it into images. Would there be a way to force LyX to convert every equation into an image?

[TUG]: http://www.tug.org/utilities/texconv/textopc.html
[MathType]: http://www.dessci.com/en/products/mathtype/
[Aurora]: http://elevatorlady.ca/index.html
[TeX2Word 2.0]: http://www.chikrii.com/products/tex2word/
[TeX2Word]: http://www.chikrii.com/products/tex2word/
[TeX2Word 3.0]: http://www.chikrii.com/products/tex2word/
[latex2rtf]: http://latex2rtf.sourceforge.net
[JPG-PNGTOEPS]: https://www.assembla.com/code/ainotes/subversion/nodes/tools/JPG-PNGtoEPS?rev=13
