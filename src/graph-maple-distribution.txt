
# This Maple worksheet plots the standard normal cumulative distribution.
#
# Plots are not displayed interactively, but saved directly to file.
#
# The crop_plots flag controls cropping whitespace bounding PostScript plots.
# Cropping uses executables typically available to TeX distributions:
# a) epstopdf,
# b) pdfcrop,
# c) pdftops.
#
# It's assumed the executables are available via the operating system's PATH environment variable.
# Note too, pdfcrop assumes Perl is installed.
#
# Toggle this flag to true to activate cropping, false to deactivate.
crop_plots := true:


# Function definition.

CropPlot := proc(path_prefix::string, file_name_eps::string, $)

  description "Crop the bounding box of a Maple plot.":

  # Input parameters:
  #
  #        name: path_prefix
  #   data type: string
  #       notes: A file path specifying the folder of the target plot.
  #              It's assumed it ends in a directory separator character.
  #              I.e. "\" or "/", as appropriate for the operating system.
  #
  #        name: file_name
  #   data type: string
  #       notes: A file name specifying the target EPS file holding the plot.
  #              N.B. EPS is Encapsulated PostScript.

  # Local variables setup.
  local file_name_eps_cropped, file_name_pdf, file_name_pdf_cropped:

  # Convert PostScript plots to PDF.
  ssystem(cat("epstopdf \"", path_prefix, file_name_eps, "\"")):

  # Get a plot file name, with '.eps' swapped for '.pdf'.
  file_name_pdf := cat(file_name_eps[1 .. -4], "pdf"):

  # Compose a cropped test plot file name.
  # Add a "cropped" distinguishing string in the file name.
  file_name_pdf_cropped := cat(file_name_pdf[1 .. 4], "cropped_", file_name_pdf[5 .. ]):

  # Crop the PDF.
  ssystem(cat("pdfcrop \"", path_prefix, file_name_pdf, "\" \"", path_prefix, file_name_pdf_cropped, "\"")):
  FileTools:-Remove(file_name_pdf, force = true):

  # Convert back to PostScript, with a distinguishing file name.
  file_name_eps_cropped := cat(file_name_pdf_cropped[1 .. -4], "eps"):
  ssystem(cat("pdftops -eps \"", path_prefix, file_name_pdf_cropped, "\" \"", path_prefix, file_name_eps_cropped, "\"")):

end proc:


# Define plot colours.
clr_cool_light_grey := ColorTools:-Color([203/255, 207/255, 210/255]):
clr_cool_very_dark_grey := ColorTools:-Color([118/255, 123/255, 141/255]):
clr_black := ColorTools:-Color([0, 0, 0]):

# Define plot fonts.
plot_font := ["Libertine", 20]:
plot_font_small := ["Libertine", 18]:
font_name := "_libertine":

# Define plot properties.
type_eps := ".eps":
plot_export_options := "height = 3.5cm, noborder, width = 3.5cm":

# Define plot styles.
#
# Common plot style elements.
plot_style_common := x = -3.8 .. 3.8, y = -0.05 .. 1.05, axes = boxed, axesfont = plot_font_small, colour = clr_black, font = plot_font, labelfont = plot_font:
#
# Axes colour: default (black).
plot_style_default := plot_style_common, axis = [gridlines = [colour = clr_cool_light_grey, linestyle = dash]]:
#
# Axes colour: very dark grey.
plot_style_very_dark_grey := plot_style_common, axis = [colour=clr_cool_very_dark_grey, gridlines = [colour = clr_cool_light_grey, linestyle = dash]]:
#
# Axes colour: white.
plot_style_white := plot_style_common, axis = [colour="White", gridlines = [colour = clr_cool_light_grey, linestyle = dash]]:
#
list_styles_active := ["_default", "_very_dark_grey", "_white"]:

# Define function to plot.
func_sn_cumulative_distribution := Statistics:-CumulativeDistributionFunction(Normal(0.0, 1.0), x):

# File name management.
file_name_eps := StringTools:-StringBuffer():
if crop_plots = true then
  path_prefix := cat(currentdir(), "\\"):
end if:

# Enable plotting from within nested loops.
printlevel := 2:

# If plot cropping is active, test if it's viable.
if crop_plots = true then

  # Generate a dummy plot.
  dummy_eps := "img_dummy_test.eps":
  plotsetup(postscript, plotoutput = cat(path_prefix, dummy_eps)):
  plot(x, x = -5 .. 5, y = -5 .. 5):

  # Test epstopdf.
  # It's expected that a pdf version of the dummy eps plot will be created.
  if ssystem(cat("epstopdf \"", path_prefix, dummy_eps, "\""))[1] <> 0 then

    # Assume a non-zero return value is failure.
    crop_plots := false:
    print("Validation failed for epstopdf. Plot cropping disabled."):

  else

    # Get the name of the file created by epstopdf.
    # I.e., swap the dummy plot extension from ".eps" to "pdf".
    dummy_pdf := cat(dummy_eps[1 .. -4], "pdf"):

    # Compose a cropped test plot file name.
    # Add a "cropped" distinguishing string in the file name.
    dummy_pdf_cropped := cat(dummy_pdf[1 .. 4], "cropped_", dummy_pdf[5 .. ]):

    # Test pdfcrop.
    # It's expected that the dummy pdf plot will be cropped of bounding space.
    if ssystem(cat("pdfcrop \"", path_prefix, dummy_pdf, "\" \"", path_prefix, dummy_pdf_cropped, "\""))[1] <> 0 then

      # Assume a non-zero return value is failure.
      crop_plots := false:
      print("Validation failed for pdfcrop. Plot cropping disabled."):

      # Generally, warn about administrative privileges compromising pdfcrop,
      # UNLESS the session already has administrative privileges in Windows.
      if not (StringTools:-Search("WINDOWS", StringTools:-UpperCase(kernelopts(bindir))) > 0 and ssystem("net session")[1] = 0) then
        print("Failure of pdfcrop may be related to the session's level of administrative privileges."):
      end if:

    else
      # Test pdftops.
      # A cropped dummy eps should be generated from the cropped dummy pdf.
      dummy_eps_cropped := cat(dummy_pdf_cropped[1 .. -4], "eps"):
      if ssystem(cat("pdftops -eps \"", path_prefix, dummy_pdf_cropped, "\" \"", path_prefix, dummy_eps_cropped, "\""))[1] <> 0 then

        # Assume a non-zero return value is failure.
        crop_plots := false:
        print("Validation failed for pdftops. Plot cropping disabled."):

      else
        # Delete the cropped dummy eps created by pdftops.
        # The force parameter suppresses error messages for failed deletion.
        FileTools:-Remove(dummy_eps_cropped, force = true):
      end if:

      # Delete the cropped dummy pdf created by pdfcrop.
      FileTools:-Remove(dummy_pdf_cropped, force = true):

    end if:

    # Clean up the dummy pdf created by epstopdf.
    FileTools:-Remove(dummy_pdf, force = true):

  end if:

  # Clean up the dummy eps created by the "plot" call.
  FileTools:-Remove(dummy_eps, force = true):

end if;


# Render plots.
for i, style_active in list_styles_active do

  # Plot setup.
  file_name_eps := cat("img_sn_cumulative_distribution_style", style_active, "_font", font_name, type_eps):

  # Plot the standard normal cumulative distribution.
  plotsetup(postscript, plotoutput = file_name_eps, plotoptions = plot_export_options):
  if style_active = "_white" then
    plot(func_sn_cumulative_distribution, plot_style_white, labels = [z, Phi(z)]):
  elif style_active = "_very_dark_grey" then
    plot(func_sn_cumulative_distribution, plot_style_very_dark_grey, labels = [z, Phi(z)]):
  else
    plot(func_sn_cumulative_distribution, plot_style_default, labels = [z, Phi(z)]):
  end if:

  # Optionally, crop any bounding whitespace.
  if crop_plots = true then
    CropPlot(path_prefix, file_name_eps):
  end if:

end do;


# Reset plot setup.
plotsetup(default):
