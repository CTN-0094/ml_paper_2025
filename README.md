# CTN-0094 Modeling Paper

This is a write-up of the modeling work Ray did in 2022 and Kyle cleaned/revised in 2024.

The analyses for this can be found in https://github.com/CTN-0094/ctn0094modeling. The supplement uses the code from the that repo.  

Apr 23, 2025: project updated

**The following are Kyle's updates**
+ Updated to using R 4.5.0
+ Reran ctn0094modeling scripts to repopulate that repo's `data` directory
  - the `ctn0094modeling/data/` will be used here as data objects to render the manuscript
  - ensure you are using R 4.5.0!
+ Render the manuscript using either:
  - the GUI (graphical user interface tools) within RStudio to "knit" the document
  - terminal commands with `quarto preview paper.qmd --to elsevier-pdf --no-watch-inputs --no-browse`
+ Render the manuscript. I used `quarto render paper.qmd` in the terminal


## Important for rendering the .qmd manuscript because of caption & figure issues!!!
+ There are manual changes that are needed to happen within the *.tex file:
  1. There is a duplicate Table 1 caption:
    - remove the `\caption{` (line 599 or so) *WITHOUT* the `\label{tbl-one}`
    - in other words: *KEEP* the entry with `\label{tbl-one}`!
    - removing this (currently, lines 593-595):

      `\caption{Overall model performance, measured using area under the ROC curve, when predicting failure of treatment in a training data set of 1,858 people seeking care for Opioid Use Disorder.}\tabularnewline`
  
  2. To center Figure 2: Variable Importance Plot (VIP) for Random Forest Model, do:
    - add `\vspace*{\fill}` on lines 682 & 697 (between `\begin{landscape}` & `\end{landscape}`)
    - should look like this:

      ```
      \begin{landscape}
      \vspace*{\fill}

      \begin{figure}

      \centering{

      \pandocbounded{\includegraphics[keepaspectratio]{paper_files/figure-pdf/fig-two-1.pdf}}

      }

      \caption{\label{fig-two}Variable Importance Plot (VIP) for Random Forest
      Model.}

      \end{figure}%

      \vspace*{\fill}
      \end{landscape}
      ```
  
  3. You will now need to remake the PDF:
    - In the terminal, run these commands:
      ```bash
      pdflatex paper.tex
      ```
    - Open the PDF to verify: 
      * the second table caption is removed
      * the VIP plot is centered horizontally & vertically
      * there are *NO* bibliography contents (fixed in next step)
    
    - Rebuild the PDF again to add the biobliography & usable internal citation links:
      ```bash
      pdflatex paper.tex
      bibtex paper
      pdflatex paper.tex
      pdflatex paper.tex
      ```
      * the multiple `pdflatex paper.tex` entries are *intentional* as I have read that
        numerous renderings may often be needed:
        ```
        First pdflatex run: Creates auxiliary files with citation references
        BibTeX run: Processes these references and generates the bibliography
        Second pdflatex run: Incorporates the bibliography but may not resolve all cross-references
        Third pdflatex run: Finalizes all cross-references and ensures everything is properly linked
        ```
      
    - Open & inspect the table, images, and citation links again
