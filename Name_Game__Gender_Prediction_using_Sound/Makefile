#target: [dependency]
#<tab>recipe
ipynb:
	notedown --from markdown --to notebook NGGPUS.md > itr-gnr-pres.ipynb
ppt:
	pandoc -s NGGPUS.md --to pptx --toc --reference-doc=ppt/celestial.pptx -o NGGPUS.pptx
pdf:
	pandoc -s NGGPUS.md --to beamer -V theme:Copenhagen -o NGGPUS.pdf
doc:
	pandoc -s NGGPUS.md --to docx --reference-doc=style.docx -o NGGPUS.docx
slidy:
	pandoc -s NGGPUS.md --to slidy --self-contained -V theme=moon -o NGGPUS.html
dzslides:
	pandoc -s NGGPUS.md --to dzslides --self-contained -V theme=moon -o NGGPUS.html
s5:
	pandoc -s NGGPUS.md --to s5 --self-contained -V theme=moon -o NGGPUS.html
revealjs:
	pandoc -s NGGPUS.md --to revealjs --self-contained -V theme=moon -o NGGPUS.html
slideous:
	pandoc -s NGGPUS.md --to slideous --self-contained -V theme=moon -o NGGPUS.html
