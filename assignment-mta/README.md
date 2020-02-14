 ### Amazing Image Changer

 This command line app scans for images in ./in folder, 
 and turns them into chalk-looking images in ./out folder,
 then exits.

 It logs to two files in ./logs using Winston module

 It requires node v8.x

 The server need to have GraphicsMagick installed, so 
 you might need to do something like installing with apt:

 `apt-get install -y graphicsmagick`

 index.js is the main entrypoint

# TODO
make logging use stdout
ignore logs, in/, out/ (find where out is set)
bind mount in/out dirs to pull in images from host
find and set env variable

# Success:
in and out dirs should populate the image
--env should overwrite any envs set in dockerfile
no .gif files in image
docker logs should show winston by default
