## Troubleshooting
Q: After opening the project with Evercoast's plugin, Unreal just crashed.
A: It might be caused by your CPU lacks of AVX2 instruction set. If you see something like "Unhandled Exception: EXCEPTION_ILLEGAL_INSTRUCTION" in the crash reporter window, then it might very likely be the case. Evercoast's ECV format needs AVX2 to decode. However, if you only going to work with ECM files, then you can still use the plugin. However you'll need to disable the Evercoast plugin, or change the opening map to workaround it.

Q: The local ecv/ecm file doesn't load/has loading errors
A: It is recommended that you put ecv/ecm on a local drive and inside of the project directory, e.g. `Content/ECMs` will be a good choice. After that, re-import the ecv/ecm again and apply the asset to the actor.

Q: The ecv/ecm won't play sound
A: Please refer to the `Recipe` - `Enabling Audio` section. Generally adding a `AudioComponent` on the actor will output sound recorded with the volcap.

Q: The ecm shows up but freezes/won't move
A: Some of the media player implementations in Unreal aren't suitable for ECM playback. Please make sure that `Electra Player` is enabled in the `Plugins` settings.

Q: The ecv/ecm worked fine in editor but doesn't appear in the standalond build
A: This can happen when you move the imported .uasset around outside of Unreal Engine. Please do the following and build the game again:
	- For the imported ecv/ecm, do right click > `Force Invalidate Content`
	- For the same ecv/ecm, do right click > `Validate Content`
	- You should see something like `EvercoastEditorLog: Validate data URL [YourFilename].ecm : PASSED`, indicating the content was successfully read and rehashed.
	- Save the asset by Ctrl-S.

Q: The ecv/ecm seems imported fine but won't display
A: Please make sure that you have set the `Renderer Actor` on the `EvercoastStreamingReaderComp` component, and have chosen the right `ECVMaterial` in the `EvercoastRendererSelectorComp`. Also don't forget to assign `ECVAsset` on the `EvercoastStreamingReaderComp` component to the imported volcap.

Q: I moved the ecm/ecv files around, and the volcaps can no longer play in Unreal.
A: The ECM or ECV files need to be in the same place throughout the project development. Because during the asset importing, it wasn't actually imported as an Unreal object, but instead creating a redirector so that Evercoast's code can find it. If you moved the ECM or ECV files, you'll need to update the associated .uasset file and edit its `Data URL` field to reflect your changes then save.