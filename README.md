Requirements:

    1) Windows
    2).Net 4 framework
    3) iTunes must be installed
    4) Mounted image of MAC computer (adding instructions on how to do this later)

Launch the program.  Four things will need to be set before you can begin parsing:

Mounted MAC Drive

This will be the Root folder of your mounted MAC Image

Location to Save Report

	Self explanatory. The report will be a text document.  Best viewed with Notepad ++ or other similar program.

Name of User's Directory

	This will be populated with the users directories once the root folder is selected. A user must be selected to continue.

Select Plugins to Run

	Choose which plugins you would like to run. You can run all of them, or multiple select.

About the Plug-ins:

There are about 45 plug-ins so far.  Each plugin can contain one or more plist files.  I have tried to fit each plug-in into a category:

   Initial Data Gathering : These are items usually needed to begin working on the case, or data that is always included in the report.  Time Zone, OS Version, Computer Name etc

   System Information:    Attached Media, Global Preferences etc

   Network Information:  Wireless Networks, Firewall, Network Configuration etc.

   Applications: Various application Preferences, Recent Items etc

   Internet, Email, Chat: Safari, Chrome, Email and Chat Clients, etc

I have also pulled out some plugins on their own.  Maybe these are things I view often or I just prefer to run quickly, like Safari.
Making Plug-Ins/Plug-In Format

The plug-in files are written in XML format, and located in the PlugIns folder.  Each plug-in file contains one or more plist files. This allows for plist files to be grouped according to your needs.  For example, there are several plist files containing information for TimeMachine, the backup utility included in MAC OS X.  As such, the plug-in for TimeMachine contains  three entries, one for each plist file.  Maybe you want a plug-in that parses all remote applications.  You could create one plug-in that contains plist files for LogMeIn and Remote Desktop. Plist files can contain multiple keys. Maybe you are only interested in one or two keys from the plist file.  In this case, you can specify certain keys to be parsed. The easiest way to view the format is probably to just view some of the plug-in files with your favorite text editor.  However, I have displayed several examples below followed by an explanation of the tags and possible values.

Examples:

Plist in user directory:
<plugin name = Tunes>

	<plist>

		<pldesc>iTunes Preferences</pldesc>
		<location>\Library\Preferences\com.apple.iTunes.plist</location>
		<type>binary</type>
		<isUserDirectory>True</isUserDirectory>
		<keys/>

	</plist>

</plugin>

Plist List NOT in User Directory:

<plugin name = FireWall Setting>

	<plist>

		<pldesc>FireWall Settings</pldesc>
		<location>\Library\Preferences\com.apple.alf.plist</location>
		<type>binary</type>
		<isUserDirectory>False</isUserDirectory>
		<keys/>

	</plist>

</plugin>

Plist with specific keys

<plist>

	<pldesc>Computer Name</pldesc>
	<location>\Library\Preferences\SystemConfiguration\preferences.plist</location>
	<type>binary</type>
	<isUserDirectory>False</isUserDirectory>
	<keys>

		<key>ComputerName</key>

	</keys>

</plist>

Plugin File with multiple plist entries:

<plugin name = Time Machine and Snapshot>

	<plist>

		<pldesc>Time Machine Preferences</pldesc>
		<location>\Library\Preferences\com.apple.TimeMachine.plist</location>
		<type>binary</type>
		<isUserDirectory>False</isUserDirectory>
		<keys/>

	</plist>
	<plist>

		<pldesc>Last Backup Date</pldesc>
		<location>\private\var\db\.TimeMachine.Results.plist</location>
		<type>binary</type>
		<isUserDirectory>False</isUserDirectory>
		<keys/>

	</plist>
	<plist>

		<pldesc>Snap Shot Dates</pldesc>
		<location>\private\var\db\com.apple.TimeMachine.SnapshotDates.plist</location>
		<type>binary</type>
		<isUserDirectory>False</isUserDirectory>
		<keys/>

	</plist>
	
</plugin>

Tag Definitions:

<plugin name> This will be seen in the report header for the plugin file.

<pldesc> The plist description. This will be shown in the report as that particular plist's description.

<location> Location of the plist file from the root folder. If this is a user directory, LEAVE OFF the \Users\%User%\. This will be supplied at runtime by the program.

<type>  binary for a binary plist, xml for an xml.  If unknown, use binary.
<isUserDirectory> True of False. Is this plist file located in a user directory? if so, True otherwise False

<keys>If all keys are to be parsed, leave empty <keys/>

<key> If you do not want all the keys parsed in a file, put in the key name here as a child under <keys>

PlugInsToRun
The PlugInsToRun.txt file calls the plug-in files and populates the list box where you choose which plug-in files to run.  After you make a plug-in, you must add it to one of the existing entries or make a new entry, or both. If you are going to add it as a single entry, I would recommend adding it to one of the Main Categories as well.

<group name>  This will be seen in the listbox
<description>    Description
<file> The plug in file name, including the extension

Entry with multiple files called:
<group name = Internet, Email, Chat>

	<description>Various Internet Artifacts</description>
	<plugins>

		<file>iChat.txt</file>
		<file>iCloud.txt</file>
		<file>safari.txt</file>
		<file>Mail.txt</file>
		<file>Skype.txt</file>
		<file>Twitter.txt</file>
		<file>RSS.txt</file>
		<file>Chrome.txt</file>
		<file>Safari.txt</file>

	</plugins>

</group>

Entry with one file called:

<group name = Mail>

	<description>Mail Items</description>
	<plugins>

		<file>Mail.txt</file>

	</plugins>

</group>
