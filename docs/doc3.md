---
id: doc3
title: This is Document Number 3
---

# Uniforms
So far we have seen how the GPU manages large numbers of parallel threads, each one responsible for assigning the color to a fraction of the total image. 

각 병렬 쓰레드는 Although each parallel thread is blind to the others, we need to be able to send some inputs from the CPU to all the threads. Because of the architecture of the graphics card those inputs are going to be equal (uniform) to all the threads and necessarily set as read only. 

즉, 각 쓰레드들은 같은 데이터를 받는다는 말입죠. 그 데이터는 읽을수는 있으나 바꿀 수는 없어유.
이 입력값들을 unifirm 이라고 하고 다수의 타입들을 지원합니다:
float, vec2, vec3, vec4, mat2, mat3, mat4, sampler2D and samplerCube. 

Uniforms are defined with the corresponding type at the top of the shader right after assigning the default floating point precision.

```c
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;  // Canvas size (width,height)
uniform vec2 u_mouse;       // mouse position in screen pixels
uniform float u_time;       // Time in seconds since load
```

유니폼을 CPU와 GPU 사이의 다리라고 생각하면 됩니다. The names will vary from implementation to implementation but in this series of examples I’m always passing: 

uniforms | 설명
-------|-----------------------------------------
vec2 <b><i>u_resolution</i></b> | billboard size where the shader is being drawn
vec2 <b><i>u_mouse</i></b> | mouse position inside the billboard in pixels
float <b><i>u_time</i></b> | time in seconds since the shader started

I’m following the convention of putting u_ before the uniform name to be explicit about the nature of this variable but you will find all kinds of names for uniforms. For example ShaderToy.com uses the same uniforms but with the following names:

uniform | dd
--------|---------------------------------------
uniform vec3 <b><i>iResolution</i></b> | viewport resolution (in pixels)
uniform vec4 <b><i>iMouse</i></b> |  mouse pixel coords. xy: current, zw: click
uniform float <b><i>iTime</i></b> | shader playback time (in seconds)

Enough talking, let's see the uniforms in action. In the following code we use `u_time` - 셰이더가 시작하고 the number of seconds since the shader started running - together with a sine function to animate the transition of the amount of red in the billboard.

```c
#ifdef GL_ES
precision mediump float;
#endif

uniform float u_time;

void main() {
    gl_FragColor = vec4(abs(sin(u_time)),0.0,0.0,1.0);
}
```

As you can see GLSL has more surprises. The GPU has hardware accelerated angle, trigonometric and exponential functions. Some of those functions are: sin(), cos(), tan(), asin(), acos(), atan(), pow(), exp(), log(), sqrt(), abs(), sign(), floor(), ceil(), fract(), mod(), min(), max() and clamp().

Now it is time again to play with the above code.

Slow down the frequency until the color change becomes almost imperceptible.

Speed it up until you see a single color without flickering.

Play with the three channels (RGB) in different frequencies to get interesting patterns and behaviors.
gl_FragCoord
In the same way GLSL gives us a default output, vec4 gl_FragColor, it also gives us a default input, vec4 gl_FragCoord, which holds the screen coordinates of the pixel or screen fragment that the active thread is working on. With vec4 gl_FragCoord, we know where a thread is working inside the billboard. In this case we don't call it uniform because it will be different from thread to thread, instead gl_FragCoord is called a varying.

```c
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main() {
    vec2 st = gl_FragCoord.xy/u_resolution;
    gl_FragColor = vec4(st.x,st.y,0.0,1.0);
}
```

In the above code we normalize the coordinate of the fragment by dividing it by the total resolution of the billboard. By doing this the values will go between 0.0 and 1.0, which makes it easy to map the X and Y values to the RED and GREEN channel.

In shader-land we don’t have too many resources for debugging besides assigning strong colors to variables and trying to make sense of them. You will discover that sometimes coding in GLSL is very similar to putting ships inside bottles. Is equally hard, beautiful and gratifying.



Now it is time to try and challenge our understanding of this code.

Can you tell where the coordinate (0.0, 0.0) is in our canvas?

What about (1.0, 0.0), (0.0, 1.0), (0.5, 0.5) and (1.0, 1.0)?

Can you figure out how to use u_mouse knowing that the values are in pixels and NOT normalized values? Can you use it to move colors around?

Can you imagine an interesting way of changing this color pattern using u_time and u_mouse coordinates?
After doing these exercises you might wonder where else you can try your new shader-powers. In the following chapter we will see how to make your own shader tools in three.js, Processing, and openFrameworks.
