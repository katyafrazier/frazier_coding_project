# frazier_coding_project

The purpose of this code is to automate the analysis process of high-performance liquid chromatography (HPLC) tandem-mass spectrometry (MS/MS) data.

In the Chang lab, we often want to measure levels of certain metabolites in intestinal content samples, from both mice and humans. Metabolite levels can often inform on changes in physiology, such as an influx of gut microbes that produce a particular compound. We utilize HPLC-MS/MS to measure metabolite levels, which ionizes samples and sorts compounds by their mass-to-charge ratio, measured in the form of peak area.

Output from the HPLC-MS/MS machine consists of an individual file for each compound and sample. Because most experiments measure several compounds in many samples, as well as water controls and standards for each compound, this results in dozens of output files. After data extraction from the machine, all analysis is usually done manually in Excel and Prism, by members of my lab. This is time consuming and very monotonous, which is why I chose this process for automation.

The scripts for this analysis, one in Python and one in R, enable the user to extract the relevant data from each raw file into a dataframe, normalize the sample data to standard curve slope values (by linear regression), display the data in a bar plot, and save the data as a final .csv file. The example data included in this project contains water controls, standards, and three human ileostomy samples (52_A1, 56_A1, EL_A1) for 7 metabolites (creatine, tryptophan, serotonin, nms, kynurenine, ipa, and hippuric acid). Each raw data file name contains the sample name (i.e. 52_A1), Pos, the metabolite name (i.e. creatine), and the run number (i.e. r1).

The Python script requires a folder of input files (rawdata_folder) and an output .csv file name (output_data) to run the hplcmsms_dict_csv function. The raw data files in the "rawdata" folder are an example of the format of files that the script requires for correct analysis. Each .CSV file in the "coding_project_rawdata" repository contains the machine's attempts at MS measurement; the script extracts the "Accepted" measurement attempt with the largest peak value. The "python_data.csv" file is an example of output from this script, which is required as input for the R script.

The R script requires 3 input files (stdfile, mgfile, pythonfile), an output plot file name (plot), and an output data file name (output) to run the "hplc_msms_analysis" function. The "stdfile" input .csv should contain the concentration of each standard for each metabolite, as shown in the file "std_concentrations.csv". The "mgfile" input .csv should contain the milligrams of each original sample for eventual normalization, as shown in the file "mg_sample.csv". The "pythonfile" input .csv should contain the extracted data from the Python output, as shown in the file "python_data.csv". The "plot" .pdf is the output bar ggplot displaying the final data, as shown in the file "plot.pdf". The "output" .csv contains the final data, as shown in "output.csv".

Within the "hplc_msms_analysis" function, there are a few analysis steps that I want to explain. 1) Each sample is run twice, hence the "r1" or "r2" in each file name. The area values for each pair of files is simply averaged. 2) The Water control area values are subtracted from sample area value (both experimental samples and standards) to normalize the values. 3) Each set of standards for each metabolite is used to conduct a linear regression, and the slope values for each best fit line is used in the following sample analysis. 4) The NMS metabolite is different from the others in that it is always added to every sample at a specific amount to measure extract efficiency. Once extraction efficiency is calculated, all other metabolite samples are normalized to it in order to calculate the actual amount of each metabolite in the original samples.
