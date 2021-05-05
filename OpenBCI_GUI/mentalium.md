# revision for Mentalium 
## application name 
add a global appName in OpenBCI_GUI.PDE : 
String appName = "Mentalium";  // to replace text appName+"
search every occurence of string "OpenBCI GUI" , replace by appName+"
( 56 replacement)

find main title :  surface.setTitle("OpenBCI GUI " +
change by 
### remove logo in top bar 
Create a virgin image named logo_white_empty.png 
replace setting of logo_white 
    //logo_white = loadImage("logo_white.png");
    logo_white = loadImage("logo_white_empty.png");



## add filter 
dataProcessingBPArray has a list of names for  filters in the class SessionSettings, but it is not used

BandPassranges is an explicit enum in FilterEnums.pde and the label are dynamically constructed by getDescr()
=> it's enough to add a new filter in the enum list. We add 1-30 at the end to avoid renum others.
line 50 :      
    None(5, null, null),
    OneToThirty(6,1.0d,30.0d);

## add rs323
adding the CP2102 option for the COM list.
line 586 ControlPanel.pde
 private LinkedList<String> getCytonComPorts() {
        final String[] names = {"FT231X USB UART", "VCP", "CP2102"};

## change logo 
Add logo-mentalium_nobackground-1000.png in data folder
Dans openBCI_GUI.PDE around line 427 change source image
 //cog = loadImage("cog_1024x1024.png");
 cog = loadImage("logo-mentalium_nobackground-1000.png");

## indicate a special version 
OpenBCI_GUI line 69 : ( don't change previous 5.0.4 )
//String localGUIVersionDate = "March 2021";
String localGUIVersionDate = " /pep-inno - 05/05/21";

## remove options in upper bar 
### remove tutorial option 
in TopNav.pde 
comment lines 68+ after createDebugButton
comment lines 207+ after if (previousSystemMode != systemMode) { up to corresponding }
comment lines 291+ after debugButton.setPosition... up to if (systemMode)
** line 305 : to be checked 
