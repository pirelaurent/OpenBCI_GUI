# Revision for Mentalium Part 1 
## application name 
Add a global *appName* in OpenBCI_GUI.PDE :    
```String appName = "Mentalium";  // to replace text appName+"   ```   
search every occurence of string "OpenBCI GUI" , replace by appName+"   
( 56 replacement)   

find main title :  surface.setTitle("OpenBCI GUI " +   
change by : ```surface.setTitle(appName+" "+  ```  

### remove logo in top bar 
Create a virgin image named logo_white_empty.png    
replace setting of logo_white 
``` java  
    //logo_white = loadImage("logo_white.png");   
    logo_white = loadImage("logo_white_empty.png");   
```
## add filter 
dataProcessingBPArray has a list of names for  filters in the class SessionSettings, but it is not used   

*BandPassranges* is an explicit enum in FilterEnums.pde and the label are dynamically constructed by getDescr()    
=> it's *enough to add a new filter in the enum list*.    
We add 1-30 at the end of list (position 6) to avoid to renum others standards. (around line 50 ):    
``` java    
    None(5, null, null),
    OneToThirty(6,1.0d,30.0d);
```

## add rs323
adding the CP2102 option for the COM list.    
line 586 ControlPanel.pde    
```` java
 private LinkedList<String> getCytonComPorts() {
    final String[] names = {"FT231X USB UART", "VCP", "CP2102"};
````
## change logo 
Add *logo-mentalium_nobackground-1000.png* in data folder    
Dans openBCI_GUI.PDE around line 427 change source image    
``` java 
//cog = loadImage("cog_1024x1024.png");
 cog = loadImage("logo-mentalium_nobackground-1000.png");
```

## indicate a special version 
OpenBCI_GUI line 69 : ( don't change previous 5.0.4 )
```java 
//String localGUIVersionDate = "March 2021";
String localGUIVersionDate = "-00-pla";
```
## remove tutorial option in upper bar 
in TopNav.pde    
comment lines 68+ after createDebugButton   
comment lines 207+ after if (previousSystemMode != systemMode) { up to corresponding }   
comment lines 291+ after debugButton.setPosition... up to if (systemMode)   
** line 310 : to be checked if some effect with tutorialSelector.screenResized();    
   

# Revision for Mentalium Part 2  
## Adjust colors used by app 
define mentalium colors in a list of constant     
change default value of colors line starting with :      
```final color OPENBCI_DARKBLUE = DEEP_DARK_RED;  ```
## create new entries of colors 
Some colors are directly set in code and not in setup through named variable      
Add some :    
*SELECTED_ITEM_BG*   and replace in  if (i == activeItem) of customCP5Class   
      menu.fill(SELECTED_ITEM_BG);//menu.fill(184, 220, 105, 255);   
*SELECTED_ITEM_BORDER*  and replace in  if (i == activeItem) of customCP5Class    
    menu.stroke(SELECTED_ITEM_BORDER);//menu.stroke(184, 220, 105, 255);   
*HOVER_ITEM*    
 menu.fill(HOVER_ITEM); //menu.fill(127, 134, 143);   
final color *BACKGROUND_LIST* = BLUE_MENTALIUM; // replace all .setColorBackground(color(31,69,110))   

## retracting widgets 
Mentalium needs only  timmes series and power band    
To avoid side effects, we don't remove these widgets, we just remove them from list of available widgets.   
in *void setupWidgets(PApplet _this, ArrayList<Widget> w){* of WidgetManager, comment entries : 

```java
// addWidget(w_fft, w);  
//addWidget(w_accelerometer, w);
//addWidget(w_playback, w);
//addWidget(w_ganglionImpedance, w);
//addWidget(w_networking, w);
//addWidget(w_headPlot, w);
//addWidget(w_emg, w);
//addWidget(w_spectrogram, w);
//addWidget(w_pulsesensor, w);
//addWidget(w_digitalRead, w);
//addWidget(w_analogRead, w);
//addWidget(w_packetLoss, w);
//addWidget(w_template1, w);
```

just leave :
``` java 
addWidget(w_timeSeries, w);
addWidget(w_bandPower, w);
```  
## removing other board and renaming cyton 
in *BoardBrainFlowStreaming.PDE*

``` java 
public enum BrainFlowStreaming_Boards
{
    CYTON("Mentalium", BoardIds.CYTON_BOARD), // CYTON("Cyton", BoardIds.CYTON_BOARD),
    //CYTONDAISY("CytonDaisy", BoardIds.CYTON_DAISY_BOARD),
    //GANGLION("Ganglion", BoardIds.GANGLION_BOARD),
    //GALEA("Galea", BoardIds.GALEA_BOARD),
    SYNTHETIC("Synthetic", BoardIds.SYNTHETIC_BOARD);
 ```   
in ControlPanel.PDE    
in *private void createDatasourceList*    comment : 
``` java
        if (galeaEnabled) {
           // sourceList.addItem("GALEA (live)", DATASOURCE_GALEA);
        }
        sourceList.addItem("MENTALIUM (live)", DATASOURCE_CYTON); //sourceList.addItem("CYTON (live)", DATASOURCE_CYTON);
        //sourceList.addItem("GANGLION (live)", DATASOURCE_GANGLION);
        sourceList.addItem("PLAYBACK (from file)", DATASOURCE_PLAYBACKFILE);
        //sourceList.addItem("SYNTHETIC (algorithmic)", DATASOURCE_SYNTHETIC);
        sourceList.addItem("STREAMING (from external)", DATASOURCE_STREAMING);
        sourceList.scrollerLength = 10;
    // add empty entry as pb with scroller that flicker 
        sourceList.addItem("", 9999);
        sourceList.addItem("", 9999);   
```   
## default saving file 
change name OpenBCI in : 
``` java
       //createODFButton("odfButton", "OpenBCI", dataLogger.getDataLoggerOutputFormat()...
        createODFButton("odfButton", "CSV(ODF)", dataLogger.getDataLoggerOutputFormat()...
```     
find name of file recordings directory :   
``` settings.setSessionPath(directoryManager.getRecordingsPath()+ appName+"Session_" + _sessionName + File.separator);```   

Name of raw files    
```fname += appName+"-RAW-";//fname += "OpenBCI-RAW-";```


## notes 
Risk with brainflow.jar :  A new version of OpenBCI Github can come with new brainflow.jar (or others).
The libray folder in the source code  D:\Pep-inno2020\git_project\OpenBCI_GUI\OpenBCI_GUI\libraries   
is used when running in development mode  (VSC)  
But librairies used in compiling with Processing are rather in  C:\Users\pirla\Documents\Processing\libraries 
