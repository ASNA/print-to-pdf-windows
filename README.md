This class prints an ASNA Print File to either a physical printer 
or the "Microsoft Print to PDF" print driver in a Windows application. 

There are special considerations for using the MS PDF driver with a 
Web application. [See this ariticle for using the MS PDF driver 
with an AVR web app.](https://www.asna.com/en/kb/windows-native-pdf-driver)

I like to isolate printer code in a class for each print file. In this example, the 
`CustomerReport` does that. You don't have to use a class to use the techniques 
shown in this example. You can copy code from the class and copy it inline 
into your code. That said, one of these days you'll be very grateful then if you 
take the time to separate this code initially! 
 
Use the `CustomerReport` constructor that requires two arguments to print to a physical 
printer or use the constructor that requires three arguements to print
to a PDF file. 
                                                                                          
To print to a physical printer:

```
  DclFld cr Type(CustomerReport) 	
  DclFld PrinterName Type(*String) 
  DclFld UsePrintSetup Type(*Boolean) 
 
  PrinterName = "HP LaserJet 1020 (redirected 2)"
  UsePrintSetup = *True 
  // If UsePrintSetup is *True then printer setup is shown.

  cr = *NEW CustomerReport(PrinterName, UsePrintSetUp) 
  cr.Print()
  Try 
      cr.Print()
      MsgBox String.Format("File printed") 
  Catch Type(Exception) Name(ex)
	  MsgBox String.Format("ERROR :" + ex.Message) 
  EndTry 
```   

To print to a PDF file with the MS native PDF print driver:

```
  DclFld cr Type(CustomerReport) 	
  DclFld PrinterName Type(*String) 

  cr = *NEW CustomerReport(PrinterName) 

  DclFld PrinterName Type(*String) 
  DclFld PDFOutputFolder Type(*String) 
  DclFld PDFFileName Type(*String) 
  
  PrinterName = "Microsoft Print to PDF"
  PDFOutputFolder = "C:\Users\thumb\Documents\projects\avr\pdf-files"

  // Existing PDF files are overwritten! 
  PDFFileName = "test.pdf"

  DclFld cr Type(CustomerReport) 	
		
  cr = *NEW CustomerReport(PrinterName, PDFOutputFolder, PDFFileName) 
  Try 
      cr.Print()
      MsgBox String.Format("File {0} created in {1}", PDFFileName, PDFOutputFolder) 
   Catch Type(Exception) Name(ex)
      MsgBox String.Format("ERROR :" + ex.Message) 
   EndTry 
```   

When printing to the MS native PDF driver, the printer name
must be:

    Microsoft Print to PDF

If you have trouble with this code, use MS Word to save a sample document 
the MS PDF driver to ensure the driver is available. It may need
to be added with "Turn Windows features on or off."

You cannot show the printer setup in a Web app. The entire printer configuration must 
specified programmatically for Web apps. 