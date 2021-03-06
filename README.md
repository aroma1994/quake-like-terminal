# quake-like-terminal
How to make quake-like terminal with openbox

* Use together with your usual terminal but with different appearance settings
* You can reopen this terminal on any of your desktops
* And it saves history after reopening

![quake-like-terminal](quake-like-terminal.gif)

## Dependencies

* `openbox`
* `bash`
* `termite`
* `xdotool`
* `wmctrl`


## Configuration

### Add these strings to \<keyboard>...\</keyboard> section in openbox/rc.xml:

```xml
<keybind key="C-e">
	<action name="If">
		<query>
			<title>quaketerm</title>
		</query>
		<then>
			<action name="Iconify"/>
		</then>
		<else>
			<action name="Execute">
				<command>sh -c "if [[ `wmctrl -l | awk '/quaketerm/ {print $1}'` ]]; then xdotool set_desktop_for_window $(wmctrl -l | awk '/quaketerm/ {print $1}' | head -1) $(wmctrl -d | grep '*' | cut -d ' ' -f1) windowactivate $(wmctrl -l | awk '/quaketerm/ {print $1}' | head -1); else `termite -t quaketerm --name="quaketerm"`; fi"</command>
			</action>
		</else>
	</action>
</keybind>
```


### Add these strings to your \<applications>...\</applications> section in openbox/rc.xml file:

```xml
<application name="quaketerm">
	<layer>above</layer>
	<fullscreen>no</fullscreen>
	<decor>no</decor>
	<maximized>no</maximized>
	<position>
		<x>0</x>
		<y>0</y>
	</position>
	<size>
		<width>100%</width>
		<height>54%</height>
	</size>
	<focus>yes</focus>
</application>
```


### Reconfigure and restart openbox:

```bash
$ openbox --reconfigure
$ openbox --restart
```
