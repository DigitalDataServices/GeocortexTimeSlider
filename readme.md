# Geocortex Time Slider Component #


### Description ###

This free JavaScript library that adds the ability to embed a 'Time Slider' widget within your Geocortex Viewer for HTML5.  

![TimeSlider Widget](https://raw.githubusercontent.com/DigitalDataServices/GeocortexTimeSlider/master/imgTimeSliderWidget.jpg)

The Time Slider widget enables you to view temporal layers in a Map and to see how the layer data changes over time.  Using this widget, you can play or pause the temporal animation of the data, go to a previous time period, or advance to the next time period.

In additional to the standard functionality provided by similar Esri or Silverlight-based widgets, the [Digital Data Services](http://www.digitaldataservices.com/geocortex) Time Slider widget for the HTML5 Viewer also allows you to:

- Drag and place the widget anywhere on the map
- Horizontally resize the widget - expanding or compacting the Time Slider view in real-time
- Temporarily close and reopen the widget - giving you the ability to hide the Time Slider when it is not actively being used
- Statically or dynamically configure the temporal boundaries (year range) to adjust to changing time-aware Layer content
- Automatically persist the widget's properties and settings (such as its position, visibility, and temporal range)  - so that your Time Slider preferences are retained when a Geocortex session is closed and reopened
  
### Requirements ###

The minimum requirement is Geocortex Essentials 4.4.3 and Geocortex Viewer for HTML5 2.5.2.  The component has also been tested on Geocortex Essentials 4.5 and Geocortex Viewer for HTML5 2.6. It may run on earlier versions of Geocortex Essentials and Geocortex Viewer for HTML5 - but it has not been tested.

### Table of Contents ###
- [Installation](#installation)
- [Usage](#usage)
	- [Initial Configuration](#initial-configuration) 
	- [Hiding and Showing the Time Slider Widget](#hiding-and-showing-the-time-slider-widget) 
- [Limitations](#limitations)
- [History](#history)
- [Credits](#credits)
- [License](#license)

### Installation ###

This JavaScript module can be installed in either a Geocortex Viewer for HTML5 directory or within a specific Site's HTML5 Viewer directory.

**Geocortex HTML5 Viewer Installation** - Available to all Geocortex HTML5 Viewers.

1. Locate your Geocortex HTML5 directory, typically found at `C:\inetpub\wwwroot\Html5Viewer\`
2. Navigate to the `...\Resources\Scripts\` folder and create a new directory called `Custom-Scripts`
3. Navigate to the `...\Resources\Locales\` folder and create another new directory called `Custom-Locales`

**Site Specific Installation** - Only for a specific Geocortex HTML5 Viewer.

1. Locate your Geocortex HTML5 Site directory, typically found at `C:\Program Files (x86)\Latitude Geographics\Geocortex Essentials\Default\REST Elements\Sites\`
2. Navigate to the Site's HTML5 Virtual Directory, typically found at `...\YourSiteName\Viewers\YourHTML5ViewerName\VirtualDirectory\`
3. In the `Resources` directory create two new directories - one called `Custom-Scripts` and the other called `Custom-Locales`


**Configuration for both Viewer and Site Specific Installations**

1. Copy the `TimeSlider.js` file into the `Custom-Scripts` directory and the `TimeSlider.en-US.json.js` file into the `Custom-Locales` directory.
3. Next locate the `Desktop`, `Tablet`, and `Handheld` configuration files, typically located in the `...\Resources\Config\Default\` directory.
4. Repeat these following steps for all the configuration files that are being implemented (`Desktop`, `Tablet`, and `Handheld`):

	1. *Make a backup of the configuration file!*
	2. Open the configuration file in the file editor of your choice, such as [Notepad++](https://notepad-plus-plus.org/).
	3. Within the `libraries` section add the following code where the `uri` is the relative path to the `TimeSlider.js` file using the appropriate code for your installation:

		```
		//Path if using a Geocortex HTML5 Viewer Installation
		{
			"id":"TimeSlider",
			"uri":"Resources/Scripts/Custom-Scripts/TimeSlider.js",
			"locales":[
				{
   					"locale":"en-US",
   					"uri":"Resources/Locales/Custom-Locales/TimeSlider.en-US.json.js"
   				}
			]
		}
		```
		```
		//Path if using a Site Specific Installation
		{
			"id":"TimeSlider",
			"uri":"{ViewerConfigUri}../../Custom-Scripts/TimeSlider.js",
			"locales":[
				{
   					"locale":"en-US",
   					"uri":"{ViewerConfigUri}../../Custom-Locales/TimeSlider.en-US.json.js"
   				}
			]
		}
		```
	4. Within the `modules` section add the following code:

            {
            	"moduleName": "TimeSlider",
            	"moduleType": "dds.timeSlider.TimeSliderModule",
            	"libraryId": "TimeSlider",
            	"configuration": {
            	},
            	"views": [
                  	{
                   		"id": "TimeSliderModuleView",
	                   	"viewModelId": "TimeSliderModuleViewModel",
                   		"visible": true,
                   		"markup": "Modules/TimeSlider/TimeSliderModuleView.html",
                   		"type": "dds.timeSlider.TimeSliderModuleView",
                   		"region": "CenterRegion",
                   		"configuration": {
                   			"startDate": "January 1, 1964",
                   			"endDate": "December 31, 2014"
                   		}
                  	}
            	],
            	"viewModels": [
              		{
                   		"id": "TimeSliderModuleViewModel",
                   		"type": "dds.timeSlider.TimeSliderModuleViewModel",
                   		"configuration": {
                		}
               		}
          		]
      		},
            {
            	"moduleName": "SettingsPopup",
            	"moduleType": "dds.timeSlider.SettingsPopupModule",
            	"libraryId": "TimeSlider",
            	"configuration": {
            	},
            	"views": [
             		{
               			"id": "SettingsPopupModuleView",
                		"viewModelId": "SettingsPopupModuleViewModel",
                 		"visible": false,
                  		"markup": "Modules/SettingsPopup/SettingsPopupModuleView.html",
                 		"type": "dds.timeSlider.SettingsPopupModuleView",
                 		"region": "TopRightMapRegion",
                 		"configuration": {
                  		}
                	}
            	],
            	"viewModels": [
              		{
                 		"id": "SettingsPopupModuleViewModel",
                   		"type": "dds.timeSlider.SettingsPopupModuleViewModel",
                   		"configuration": {
                 		}
               		}
          		]
            },


	5. Save the configuration file. *These modifications should be made to all relevant configuration files.*

### Usage ###

**Initial Configuration:**

Within the `modules` code block listed above, there are two custom configuration fields: `startDate` and `endDate`.  These string-based fields should be set to the initial start and end dates ([IETF-compliant RFC 2822 timestamps](http://tools.ietf.org/html/rfc2822#page-14) or a version of [ISO8601](http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15)) that define the temporal boundaries of the Time Slider.

For example, to set the initial Time Slider date range to between the years '1900' and '2000', the inner-section of the `modules` code block would be declared as follows:

```
{
	"moduleName": "TimeSlider",
	"moduleType": "dds.timeSlider.TimeSliderModule",
	...
 	"views": [
 		{
			...
   			"type": "dds.timeSlider.TimeSliderModuleView",
	   		"region": "CenterRegion",
  			"configuration": {
         		"startDate": "January 1, 1900",
            	"endDate": "December 31, 2000"
     		}
  		}
	],
	...
}
```

**Hiding and Showing the Time Slider Widget:**

The Time Slider widget is automatically displayed during the HTML5 Viewer's initial startup sequence.  The user can then temporarily close the widget by pressing the `Close` button.  To redisplay the Time Slider widget after it has been hidden by the user, the following custom Geocortex command should be used: `ShowTimeSliderControl`.

This custom `ShowTimeSliderControl` command can be easily invoked by creating a new `I want to...` Menu Item and setting `ShowTimeSliderControl` as the `Command` (the `Command Parameter` field should be left blank).  This provides the end-user with an instantly familiar path to redisplaying the Time Slider widget after it has been hidden.

### Limitations ###

The custom Time Slider JavaScript library that has been made available on [GitHub](https://github.com/) is fully functional and can be added to any of the supported Geocortex Viewer for HTML5 listed in the [Requirements](#requirements) section.  However, the Time Slider widget, in its current implementation, is limited to only displaying Year-based temporal ranges.

While it is possible to extend the widget's capabilites to include the display of Month, Day, or even Hour-based temporal ranges, such custom modifications would need to be implemented in-house by [Digital Data Services, Inc.](http://www.digitaldataservices.com/dds-contact-dds)  

Feel free to contact [us](http://www.digitaldataservices.com/dds-contact-dds) about any custom modifications to the Time Slider widget - or to assist you in the development of any Geocortex components that you can envision.  

### History ###

2016-04-20 - Initial upload.

### Credits ###

[Digital Data Services, Inc.](http://www.digitaldataservices.com/geocortex), is a Geocortex Implementation Solution Provider and Esri Silver Business Partner that specializes in the creation, conversion, management, integration, and presentation of geospatial information. Our expertise focuses on providing simple solutions to complex business challenges and allowing our clients to leverage and explore their data in new and unique ways. As experts in research, data processing, data storage/management, data analysis, and presentation, we serve our clients by making complex analytical decisions available to everyone. Our vision is that access to geospatial information should not be a barrier to making business decisions.

[Geocortex](http://www.geocortex.com/) and [Latitude Geographics](http://www.latitudegeo.com/) are registered trademarks of Latitude Geographics Group Ltd. in the United States and Canada, and are trademarks in other jurisdictions around the world.

### License ###

Copyright © 2016 [Digital Data Services, Inc.](http://www.digitaldataservices.com/geocortex) All Rights Reserved.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
