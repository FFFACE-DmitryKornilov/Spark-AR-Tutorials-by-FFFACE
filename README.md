# Spark-AR-Tutorials-by-FFFACE
Here we store public Spark AR tutorials created by FFFACE.ME team

Randomizer filters are some of the simplest and most common Instagram effects. At the same time, it can be complicated with additional effects for the face, backgrounds and 3D objects, based on the same mechanics of randomly changing options.

In addition to this, the basic randomizer filter is a great example for getting familiar with spark AR interfaces and its node system - the patch editor.

Now, we will put together a chain of patches step by step with the help of how the randomizer mechanics are implemented. Then, we will add objects on which it will be played. At the end you will have a filter ready for uploading.

1. First, click the right mouse button (further in the text RMB) on the scene tab at the top left, and choose add>facemesh. This will automatically add a Facetracker object to the scene. The facemesh will be under the facetracker, with a slight shift to the right. This means that facemesh is a child object and the facetracker is a parent object. If we move, hide, or change the layer for the parent object, the same happens with its child objects. And to the left of the facetracker there will be an arrow, which if you click, you can collapse / expand all objects that belong to it.

By the same principle, for example, if we add a rectangle to the scene, the canvas will be automatically created as its parent object.

2. In our case, we won’t use the facemesh, but we need a facetracker to track the position of the face. We remove the facemesh and then click RMB on the facetracker and click add>plane. The plane is now a child object of facetracker, and therefore moves and rotates with the face on the screen.

3. Now the plane is in the middle of the face, in order to move it, for example, on the forehead, you can pause the filter playback, and then use the arrows in the 3D viewport to move the plane up.

4. There is another, more flexible way to make the plane move with the face. This time using the patch editor. To do this, you can add a plane by clicking RMB this time on the Camera object in the scene tab. In the 3D viewport we will see a stationary plane appear in the scene, at the point from where the camera is looking.

5. Now select the plane, and on the right side, under the Transformations section, click on the arrow icon to the left of position values. This will automatically open the patch editor under the 3D view and a yellow patch will appear there; now it controls the position of the plane. Yellow means the patch is receiving information. The yellow arrow near the position value on the right in the transformations section will now be highlighted, and editing from there will be blocked. This means that it is now only adjustable with patches.

6. Click RMB on the facetracker in the scene tab and pick create patch. This will create a ready-made bundle of patches, on the right end of which there will be four outputs with different information: Face, 3D position, 3D scale, 3D rotation. We need to click on the arrow with “3D” and drag it to the arrow on the yellow patch that controls the position of the plane. Now we will see that the plane moves with the face, but at the same time it doesn't rotate, it remains perpendicular to the direction of view, since we only connected the position, but not the rotation.

7. Now the plane is again in the middle of the face, but we can no longer move it with the help of the manipulator arrows in the viewport, since its position is controlled by patches. To move the plane up with patches, we click RMB on an empty field in the patch editor, and pick Аdd patch to add some value to the height.

8. It is important here that position includes three different values, for three axes in space, and so that the Add patch can process them separately, you need to change the type of data that it transfers. To do this, click on it, and a blue drop-down menu appears below. There you need to select vector 3. Now we connect the 3D position output from facetracker to the Add patch, and the Add patch output to the yellow position patch of the plane. Now if we change the values in the second line of the add patch, these values will be added to the position value along the corresponding axes.

9. Enter 0.1 in the Y value window, to move the plane up by 0.1

10. To place questions and answers for a randomizer on the plane, you need to create a material. To do this, select the plane on the left in the scene tab, and then click the plus icon on the right of the material section. This will create the material and it will also appear in the asset window at the bottom left.

11. Now select the material in assets, and its parameters will appear on the right. There we change the shader type to flat. The flat material does not react to light sources, and there will be no shadows or highlights. With this material, the image used as a texture will show no color distortion. In this case, the color of the material should be pure white, otherwise it will mix with the texture and affect it.

12. Now upload the texture of the image that we will use at the beginning of the effect, there will be a question on it. To do this, click on the add asset at the bottom of the assets window, and select import from computer. The texture will also appear in assets. Click on it and in its parameters on the right, check the no compression box. this will speed up the application and allow the effect to load faster. You should keep in mind that all files used in the project should in total be no more than 4MB (for instagram) and 10MB (for facebook). It is necessary to reduce the textures in advance and optimize the models to the minimum acceptable size and quality in the appropriate software.

13. Now let's load the images that will be used as answers. We'll work with them as a sequence of frames, so we'll load them in a different way. We press add asset again, but now select add animation sequence. And already in its settings on the right, we add our textures, all at once. They will appear in one line in assets. You need to select them, and check the no compression checkbox in the settings on the right.

14. Now let's add textures to our material using the patch editor. To do this, drag them from the assets to the patch editor space. We drag the question texture from the textures folder in assets. But in the case of the answer textures sequence, you don’t drag the texture icon from the textures, but the animation sequence icon. Now in the material settings, click on the arrow icon near the texture slot. This will create a patch for it. If we connect the output of  the orange texture patch to the yellow patch input, we will see an image appear in our plane on the screen.

15. However, we will need the question to be replaced by answers - so that a while after the start of recording the video, the texture of the question is replaced by the textures of the answer options. We will program it like this: add the if then else patch, and add the camera patch (to do this, you can simply drag the camera to the patch editor from the scene tab). Connect the video recording output from camera patch to the condition input of the if then else patch. Connect textures of answers to then input, and texture of question to the else input.

16. To test how it works interactively in Spark AR, temporarily replace the camera patch with the value patch, and set its type to boolean. Now by checking the box in the value, we simulate the start of video recording signal.

17. Now, if you now check the box in the value patch, the picture with the question will be changed to an animation from pictures with answers. But we don’t wan’t this to happen immediately, but a few seconds after starting to record. To do this, we will add a delay patch and insert it between value and the if then else patches. Set the delay value to 2 (seconds).
.
18. Now let's set up the animation to change the answer options. Now they are controlled by the settings, which we can see on the right by clicking on the animation sequence in the assets. Here in the settings, we can specify that the animation is looped, plays frames in random order, and the frame rate per second. However, we can’t stop the animation from using patches. The only variable we can control with patches is the current frame.

19. If we click on the arrow near the current frame, we will be able to control it with patches, while the settings in the column on the right will no longer be relevant. Now let's reproduce the animation we need using patches. To do this, we need the loop animation and random patches. The loop animation has two outputs, this is the progress of animation, and a signal that fires every time the animation ends. We will use the latter. Set the animation duration to 0.05 seconds. Now connect the looped output to randomize the input of random patch. Now, every time the animation ends (every 0.05 seconds), a signal will be sent to a random patch, and a random value from 0 to 20 will be generated there (we specify 20 because we have 20 frames in our sequence, this can be seen in the assets, in the texture folder)

20. Now we need to stop the animation after a while. To do this, we will use the delay patch again, like in the case of changing the texture of the question to the texture of answers, but this time we will set the delay to a larger value - 6 seconds. In addition, after the delay patch, we add a not patch, so when the video recording turns on, the animation will turn off (with a 6 second delay).

21. Remember to connect the output video recording to the value patch that controls our mechanics. Now the whole chain looks like this - the user starts recording a video, after two seconds the picture with the question is replaced by an animation of randomly changing answers, and after 4 seconds, this animation stops at a randomly selected option. Now, the filter is ready. You can send a test link to the device, and if everything is ok, export the file for upload.

