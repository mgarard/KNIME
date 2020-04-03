# Dynamic File Name Creation

Often, one may decide to split the data in some way and wish to save the results separately such as Bin_1, Bin_2,... And then you may want to do this on multple sets of data.  Changing every variable becomes challenging and you may accidently overwrite data.  The goal of this is to simplify.  

## Getting Started

Download the ones you wish to use or alter and change the necessary variables.  The key is updating the path table for your needs.  All the workflows are avaible above.

## WalkThrough

###### Overview
1.	Simple one variable example
    1.	Column fed
    2.	Row fed
2.	Multi-variable example
    1.	Basic: Column fed
    2.	Advanced: Row fed with loop
3.	Simplify view for expansion
    1.	Meta-Nodes
    
### Simple One Variable Example

#### Column Fed
Below is a basic construction of generating a dynamic file name using a column value as a variable.  To simplyfy the task, the one thing to modify is the filepath information in the table creator.  I personally find the row based version the most useful but often a value from a column may want to be used in file name such as 'bin1'  

The example takes a file and splits the 'bin' column into sets of 'bin1' & 'bin2'.  The 'Table Column to Variable' node defines which column to sample.  The 'Table Column to Variable' samples 'row0'.  In the multi-bin example, you'll see that the index needs to be reset to use the same node without modification because of the 'row0' sampling.  The 'Variable Expression' joins the varibles into the final expression variable, names it 'dynamicFileName' and that is fed to the file writer as a variable input.  Within file writer, the file name is set to 'dynamicFileName' variable.

![fromColumnOneBin](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromColumnOneBin.svg)

[Download Workflow](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromColumnOneBin.knwf)

#### Row Fed
Similarly, below is a basic construction of generating a dynamic file name using a row value as a variable.  The column variable addition is left out of the simple example but is added back in for the advanced examples to show how it would combine with the column example.

Similarly, the 'Table Row to Variable' node defines with row to sample.  The limitation is one row unless you make muliple splits but this issue seen in the multi-column example and is futher addressed in the advanced multi-row example. The 'Table Column to Variable' samples a single row, 'row0' of the file path 'Table Creator'.  The 'Variable Expression' and file writer are set similarly to the first example.

![fromRowSinglePathWOVar](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromRowSinglePathWOVar.svg)

[Download Workflow](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromRowSinglePathWOVar.knwf)

### Multi-Variable Example

#### Column Fed Basic Expansion

Below, the 'Table Column to Variable' and 'Variable Expression' were converted to a 'meta-node' and copied for expansion.  As mentioned earlier, the index needs to be reset in the table split to accommodate the default of 'Row0' sampling.

![fromColumnMultiBin](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromColumnMultiBin.svg)

[Download Workflow](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromColumnMultiBin.knwf)

#### Row Fed: Advanced With Loop

Below is an example of processing data and wanting to save different versions.  The 'Table Column to Variable' would sample some desired name such as library ID.  This could be more complex as is done with the row iteration.

To read all the variables in the file table, the 'Table Row To Variable Loop Start' is inserted between the 'Table Column to Variable' and the 'Varible Expression'.  The file table is fed into it and the 'Variable Loop End' collates the new variables created by the 'Variable Expressions'.  The resulting table is parsed by dropping uneeded columns which could be done much sooner and then the rows are split, converted to variables by 'Table Row to Variable' and fed to the file writer as in the first example.

![fromRowMultiPathWVar](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromRowMultiPathWVarLoop.svg)

[Download Workflow](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromRowMultiPathWVar.knwf)


### Simplify View For Expansion

The above can get very messy if you're tyring to incorporate it into a larger work flow so the Dynamic Naming and Filtering is collected into meta-nodes.

![fromRowMultiPathWVarMeta](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromRowMultiPathWVarMeta.svg)

[Download Workflow](https://github.com/mgarard/Knime/blob/master/Dynamic%20File%20Name%20Creation/fromRowMultiPathWVarMeta.knwf)

# WORK IN PROGRESS
