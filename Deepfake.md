- Use pretrained

## 1. Video Data
- Different angles
- Different lighting
- If multiple videos, use Premiere Pro to stitch together
	- Same for photo
	- Both this simplifies extraction, dn use commands for renaming
- For source, clip out with obstruction, *UNLESS CAN USE MANUAL OR XSEG?? NEED RESEARCH*
- For destination, keep all clips no matter what
	- Only after face extraction, `aligned` can be deleted (but DO NOT DELETE FACES, only unrelated faces, scenes without faces, or bad aligned faces)

## 2. Extraction

### Source Extract Images
- Use lower framerate for faster extraction

### Source Extract Faces
- Use highest png quality

### Destination Extract Images

### Destination Extract Faces
- If auto extraction not good
	- Delete bad images in debug folder
	- Run auto debug extract to manually set faces
- Or, run auto extract + fix manually edit faces that are not detectable by the software
- **Note that xseg has to run again** to create masks for these newly modified manual faces

## 3. Cleaning
## Source Images
- `<NUMBER>_<FACE_NO>` - `00000101_01`
	- Search `*_0N` to deleted unwanted faces


## 4. Training

### Backup
Throughout training stages, right before settings change should create a backup incase model collapse

- Flip SRC destination
	- Enable if src doesnt have yaw of destination, incl facial expressions at that yaw
- Uniform Yaw Distribution
	- Enable if destination has alot of side angles which reduces blurry face masks at that angle
- Blur out mask
	- Dont need enable, merge can do
	- Only enable for maybe last 30-60mins before merging, then fine-tune with merge
- Random Hue, Saturation, and Lighting
	- For Diff lighting conditions
	- E.g. dark room, maybe neon lights casted on face
	- Turn on later if needed
- Face Style Power
	- For different colored faces
	- Stablises color discrepancies when swapping face (e.g. maybe parts of swapped face is not colored evenly)
	- Possible collapse
	- Turn on later if needed
- Background Style Power
	- Make face swap surrounding mask look more natural
	- Turn on later if needed; When mask surrounding background doesn't look natural
- Color transfer
	- RCT or MKL best or SOCT (SOCT is super long to merge)
	- For different skin tones
	- But don't have to do when training, merging has the same thing (UNLESS merging Color Transfer is not effective, then can do color transfer training)
	- Best used for both faces are different skin e.g. chinese and indian
- Gradient clipping
	- Reduce chances of model collapse but sacrifice speed
	- Use with GAN


> [!Important]
> I want to reuse the Edison model
> So, idk if enabling learning dropout will affect future ones
> So I will create and use 2 backups, one with only random warp
> Then another with randomwarp + gan + gradient clipping + learning dropout training
> 
> Then, I will try another destination video project
> 
> **BASICALLY BACKUP BEFORE EACH SETTING CHANGE AND NAME THE BACKUP TO THE CHANGED SETTING**

1. random warp
2. random warp + learning rate dropout
	- Once on learning rate dropout, dont add new sources because lrd will basically prevent model from learning anymore and only focus on sharping current images, same applies for next few steps
	- Should disable if want to add more sources
1. learning dropout
2. learning dropout + gan + gradient clipping
3. apply above mentioned optional settings if needed (e.g. color transfer, face style power, etc)

## Others

### Adding Additional Source Images to Existing Project
- Should be done if face swap looks blurry for certain angles and random horizontal swap and uniform yaw doesnt fix it

### Tongues
- Exclude with XSeg, it isn't trainable

**Steps**
- Use new project (avoid overwrite)
- Paste video data_src
- Extract images
- Extract facesets
- Ctrl+a and rename all to something unique, Windows will automatically autoincrement them
- xseg them
- Then, paste into existing project facesets

## What is
- XSeg
- Gan
- Check VRAM usage
- Overclocking vram
	- benchmark before training
	- make sure backup settings setup