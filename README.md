skin.confluence_withsubcheck
============================
Sample Kodi skin for script.skinsubtitlechecker based on confluence skin
It's mainly to show the possibility of the script.skinsubtitlechecker.

##Changes done to support script.skinsubtitlechecker

* The following pictures are added to the folder media\flagging\subtitle  
	- subavailable.png  
	- subnotavailable.png  
	- subunknown.png  

* The following 5 files are modified in the 720p directory as follow :

###includes.xml

Added the folowing variables :  

```XML
<variable name="SubTitleAvailable">
    <value condition="System.HasAddon(script.skinsubtitlechecker)">$INFO[window.Property(SubTitleAvailable)]</value>
    <value></value>
</variable>
<variable name="SubTitleAvailabeleLanguage">
    <value condition="System.HasAddon(script.skinsubtitlechecker)">$INFO[window.Property(SubTitleAvailabeleLanguage)]</value>
    <value></value>
 </variable>
```

###IncludesCodecFlagging.xml

Added this new include to show subtitle images with subtitle language :  

```XML
<include name="SubtitlePresentConditions">
    <control type="group" id="1">
        <width>85</width>
        <height>35</height>
        <description>Subtitle Present Image</description>
        <visible>!IsEmpty(window.Property(SubTitleAvailableLanguage))</visible>
        <control type="image" id="1">
            <left>5</left>
            <top>0</top>
            <width>80</width>
            <height>35</height>
            <aspectratio align="left">keep</aspectratio>
		    <texture>$VAR[SubTitleAvailable,flagging/subtitle/,.png]</texture>
        </control>
        <control type="label" id="1">
		    <left>38</left>
		    <top>2</top>
		    <width>47</width>
		    <height>35</height>
		    <font>font13</font>
		    <align>left</align>
		    <label>$VAR[SubTitleAvailableLanguage]</label>
		    <textcolor>grey</textcolor>
        </control>
    </control>
</include>
```  

###MyVideoNav.xml

Added the folowing onload event to start the subtitle script in the background  :

```XML
<onload>
    RunScript(script.skinsubtitlechecker, availabereturnvalue=subavailable&notavailablereturnvalue=subnotavailable&searchreturnvalue=subunknown&backend=True)
</onload>
```  
	
###DialogVideoInfo.xml

Added the folowing onload event to start the subtitle script once :

```XML
<onload>
    RunScript(script.skinsubtitlechecker, availabereturnvalue=subavailable&notavailablereturnvalue=subnotavailable&searchreturnvalue=subunknown&year=$INFO[ListItem.Year]&season=$INFO[ListItem.Season]&episode=$INFO[ListItem.Episode]&tvshow=$INFO[ListItem.TVShowTitle]&originaltitle=$INFO[ListItem.OriginalTitle]&title=$INFO[ListItem.Title]&filename=$INFO[ListItem.FileName])
</onload>
```  

###ViewsVideoLibrary.xml

Added the following lines to each view under Media Codec Flagging Images :

```XML
<include>SubtitlePresentConditions</include>
``` 
after 
```XML
<include>VideoTypeHackFlaggingConditions</include>
```

