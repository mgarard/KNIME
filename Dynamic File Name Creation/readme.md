# Dynamic File Name Creation

Often, one may decide to split the data in some way and wish to save the results separately such as Bin_1, Bin_2,... And then you may want to do this on multple sets of data.  Changing every variable becomes challenging and you may accidently overwrite data.  The goal of this is to simplify.  

## Getting Started

Download the ones you wish to use or alter and change the necessary variables.  The key is updating the path table for your needs.

## WalkThrough

#####Overview
1.	Simple one variable example
    1.	Column fed
    2.	Row fed
2.	Multi-variable example
    1.	Basic: Column fed
    2.	Advanced: Row fed with loop
3.	Simplify view for expansion
    1.	Meta-Nodes
    
###     Simple One Variable Example

Below is a basic construction of generating a dynamic file name using a column value as a variable.  To simplyfy the task, the one thing to modify is the filepath information in the table creator.  I personally find the row based version the most useful but often a value from a column may want to be used in file name such as 'bin1'  

The example takes a file and splits the 'bin' column into sets of 'bin1' & 'bin2'.  The 'Table Column to Variable' node defines which column to sample.  The 'Table Column to Variable' samples 'row0'.  In the multi-bin example, you'll see that the index needs to be reset to use the same node without modification because of the 'row0' sampling.  The 'Variable Expression' joins the varibles into the final expression variable, names it 'dynamicFileName' and that is fed to the file writer as a variable input.  Within file writer, the file name is set to 'dynamicFileName' variable.

//add one col image here

Similarly, below is a basic construction of generating a dynamic file name using a row value as a variable.  The column variable addition is left out of the simple example but is added back in for the advanced examples to show how it would combine with the column example.

Similarly, the 'Table Row to Variable' node defines with row to sample.  The limitation is one row unless you make muliple splits but this issue seen in the multi-column example and is futher addressed in the advanced multi-row example. The 'Table Column to Variable' samples a single row, 'row0' of the file path 'Table Creator'.  The 'Variable Expression' and file writer are set similarly to the first example.

//add one row image here

# WORK IN PROGRESS
