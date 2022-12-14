## Gamepad Touch Extension
### Overview
Modern gamepads support touch functionality, e.g. Sony DualSense®5, Steam deck and Steam Controllers, and Sony DualShock®4. The Standard Gamepad specification should be updated to include touchpad functionality, and the Gamepad API should be extended so that users with touch-enabled gamepads can use touch inputs on the web.

#### Standard Gamepad
The Standard Gamepad specification describes the positions of up to 17 buttons and 4 axes that are found on a typical gamepad. To support getting touchpad values, the standard gamepad will be extended to specify multiple touches found on a modern gamepad.
 
The touchpad, if present, will be assumed to be capable of capturing multiple touches. This configuration is found on many popular gamepads including the DualSense®5. When this configuration is present, it should be exposed as the touchEvents on the corresponding gamepad.

The Gamepad Extensions draft defines a GamepadTouch which represents a single touch event on a gamepad device that supports touch input and adds an optional touchEvents attribute to the Gamepad interface.

#### Goals
  * Goal: Enable applications to consume inputs from a gamepad's touchpad.
  * Goal: For multitouch touchpads, expose information about all current touches.
  * Goal: For gamepads with multiple touch surfaces, indicate which surface generated the touch event.
  * Goal: Enable applications to reconstruct the sensor coordinates of a touch event.
  * Non-goal: Add DOM events for gamepad touchpad touches.
  * Non-goal: Control the mouse cursor with a gamepad touchpad.
  * Non-goal: Integrate gamepad touch inputs with Pointer Events API.
  * Non-goal: Define heuristics for associating touchpoints into tracks.

#### Examples
Below is an example of how this API could be used to get touches from the gamepad at index 0.
A more complete example [Gamepad API demo](https://github.com/bradleyneedham/gamepadtest)
```
var gamepads = navigator.getGamepads();
var controller = gamepads[0];
if (controller && controller.touchEvents) {
    if (controller.touchEvents.length > 0 && controller.touchEvents[0].surfaceDimensions) {
    touchpadWidth = controller.touchEvents[0].surfaceDimensions[0];
    touchpadHeight = controller.touchEvents[0].surfaceDimensions[1];
    }

    var c = document.getElementById('touchCanvas');
    ctx = c.getContext('2d');
    if(!drawMode) {
    ctx.clearRect(0,0,touchpadWidth/touchpadScale,touchpadHeight/touchpadScale);
    ctx.fillStyle = 'rgba(0,0,0,1)';
    ctx.strokeRect(0,0,(touchpadWidth/touchpadScale),((touchpadHeight-2)/touchpadScale));
    }

    for (var i = 0; i < controller.touchEvents.length; i++) {
    var touchEvent = controller.touchEvents[i];
    //normalize our touchpoints
    var touchX = Math.round((touchEvent.position[0] + 1) * (touchpadWidth / 2) );
    var touchY = Math.round((touchEvent.position[1] + 1) * (touchpadHeight / 2) );

    //draw the points
    ctx.fillStyle = 'rgba('+drawColor.red + ',' + drawColor.green + ',' + drawColor.blue+', 1)';
    ctx.fillRect(touchX/touchpadScale, touchY/touchpadScale, drawSize, drawSize);
    }
}
```



### Links
PR for Gamepad Extensions draft: [Enhance Gamepad interface description for Touch #168](https://github.com/w3c/gamepad/pull/168)
