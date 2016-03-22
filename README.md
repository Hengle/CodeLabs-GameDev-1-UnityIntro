<a name="HOLTop" />
# Introduction to Unity #

---
Task 1 - Engine and Project Overview
Open Unity
*Mention usage of folders and point to .reg script
Playmode & Playmode Tint

Task 2 - GameObject Overview

Task 3 - 2D Assets and Layers
Bring in artwork and design level

Task 4 - Creating animations

Task 5 - Physics

Task 6 - AI
We can't use NavMesh (2D isn't supported) but we can choose basic waypoints or blindly move in 
1. x or y exclusively (common classic 2d)
2. A mix of both (diagonal)

Task 7 Animation

Task 8 Builds

How to handle the resize?

CAMERA SIZE


----------------

<a name="Overview" />
## Overview ##

Unity is a major player in the cross platform game engine space with over 21 supported platforms, including Windows 10 , MacOS, Linux , Android and iOS. All of which can use the same code base. In this workshop you will builds Orks with Forks (and Knives) - a fun top down 2D game.

![alt](./Images/Orks Screenshot.png)

<a name="Objectives" />
### Objectives ###

In this module, you will see how to:

- Use Unity's Editor to design a 2D level with sprites
- Handle User Input
- Work with the Camera
- Handle Input from Keyboard


<a name="Prerequisites"></a>
### Prerequisites ###

The following is required to complete this module:

- [Visual Studio Community 2015][1] or greater.
- [Unity 5.3][2] or greater.

[1]: https://www.visualstudio.com/products/visual-studio-community-vs
[2]: unity3d.com

<a name="CodeSnippets" />
### Using the Code Snippets ###

Throughout the module document, you will be instructed to insert code blocks. For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2015 to avoid having to add it manually. 

>**Note**: Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others. Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise. Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise. You can use these solutions as guidance if you need additional help as you work through this module. Unity can open multiple instances at a time.

---

<a name="Exercises" />
## Exercises ##
This module includes the following exercise:

1.  [Getting Started : Creating your first game](#Exercise1)

Estimated time to complete this module: **60 minutes**

>**Note:** When you first start Visual Studio, you must select one of the predefined settings collections. Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options. The procedures in this module describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection. If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.

<a name="Getting Started" />
### Getting Started : Creating your first game ###

In this exercise you will create your first Unity game. 

<a name="Ex1Task1" />
#### Task 1 - Create Project ####

To get started we need to create a new Unity Project

1. Open Unity
2. Choose "New Project" 
3. Unity will reload (this is by design)
4. Next we need to import all of the artwork we have and some exisitng code. 
5. Select where you want to save the soluton. It will default to c:\users\<user>\documents\visual studio 2015\Projects
6. Click OK 

If you take a look at the code you will see allot of "using Microsoft.Xna.Framework" type using clauses... whats going on there? This is MonoGame right? Yes this is MonoGame :) It is a XNA Api compabile cross platform gaming framework. To be XNA Api compatible it needs to use the same namespaces. So if you have used XNA in the past you will feel right at home with MonoGame.


<a name="Ex1Task2" />
#### Task 2 - Rename Game1.cs ####

The default project will create a class called Game1. While you can leave this as it is, its good practice to rename this class. 

1. Right click on Game1.cs  and select Rename.
2. Type in "AlienAttackGame.cs". You should see a dialog asking if you want to rename all references in the project
3. Click Yes.

<a name="Ex1Task3" />
#### Task 3 - Add Assets ####

No game would be complete without graphics and sound. Fortunately this session comes with a complete set of assets for you to use. In order to get the maxumin performance for your game you will need to pre-process the assets into a optimized format. This can be done using the MonoGame Pipeline Tool. Its purpose is to task assets like .png/.wav and compress/optimize them for the platform you are targeting. In this case Windows 10. This will allow you to take advangate of things like texture compression which in turn will allow you to include more graphics! and who doesn't like more graphics.

1. Click on the "Content" folder in the Solution to expand it.
2. Double click on "Content.mgcb". This should open the file in the MonoGame Pipeline Tool. 
    If the file opens in the code editor. Right Click on "Content.mgcb" and click Open With. The select the "MonoGame Pipeline Tool"
3. Goto Edit->Add->Existing Folder
4. Navigate to "End\AlienAttackUniversal\Content\gfx"
5. Click Add. A dialog will pop up to ask if you want to copy or link the files. Select Copy and click ok.
6. Goto Edit->Add->Existing Folder
7. Navigate to "End\AlienAttackUniversal\Content\sfx"
8. Click Add. A dialog will pop up to ask if you want to copy or link the files. Select Copy and click ok.
9. Goto Edit->Add->Existing Item
10. Navigate to "End\AlienAttackUniversal\Content\Font.spritefont"
11. Click Add. A dialog will pop up to ask if you want to copy or link the files. Select Copy and click ok.


<a name="Ex1Task4" />
#### Task 4 - Load a Texture ####

Now that all the assets have been added to our Content.mgcb file they will be compiled and automatically included in our application package. Our next task is to use the Content manager to load a texture. The code we are about to add will be removed later but it will demonstrate how easy it is to load assets.

1. Open AlienAttackGame.cs if it is not already open.
2. Add the following code just under the SpriteBatch declaration on line 12.
```csharp
	Texture2D background;
```	
3. Navigate to the LoadContent method on line 42 and add the following code
```csharp
	background = Content.Load<Texture2D> ("gfx/bgScreen");
```

That is all there is to it. The same can be applied to SoundEffect/Meshes etc. The Content.Load method is a generic method so we just specifcy the type we are trying to load. If the types do not match a error will be raised. For example if we try to load a sound effect and pass <Texture2D> we will get an error.

<a name="Ex1Task5" />
#### Task 5 - Draw a Texture ####

To render any 2 dimensional (2D) textures we can make use of the SpriteBatch class. This class is designed to draw textures and text to the screen. You can use it to efficiently *batch* the drawing of items, this helps to make sure you are making the most productive use of the video resources. The game template creates a spriteBatch for you.

The SpriteBatch has .Begin and .End methods. You use these to define a *batch*, all the calls to the various .Draw methods between a .Begin/.End pair will be optimized and sent to the GPU as efficiently as possible. Lets draw the texture we loaded in Task 4.

1. In the AlienAttachGame.cs file, find the Draw method. It should contain the following code
```csharp
	protected override void Draw(GameTime gameTime)
       	{
            GraphicsDevice.Clear(Color.CornflowerBlue);

            // TODO: Add your drawing code here

            base.Draw(gameTime);
        }
```

2. Under the //TODO section add the following code
```csharp
            spriteBatch.Begin();
            spriteBatch.Draw(background, Vector2.Zero, Color.White);
            spriteBatch.End();
```

3. Make sure your selected build configuration is set to "Debug" "x64"
4. Hit F5 or click the Run "Local Machine" button.

Congratulations, you just loaded and rendered your first texture!

<a name="Ex1Task6" />
#### Task 6 - Draw Text ####


Drawing text is similar to drawing a texture. We first need to load a font , and then use the SpriteBatch to render the text.

1. Go back to where we declared the background Texture2D and add
```csharp
	SpriteFont font;
```

2. Go to the LoadContent method and add the following underneath were we loaded the texture.
```csharp
	font = Content.Load<SpriteFont>("Font");
```

3. Next modify the Draw code we pasted earlier to include a call to spriteBatch.DrawString just underneath the call to draw our texture.
```csharp
	spriteBatch.DrawString(font, "MonoGame Rocks!", Vector2.Zero, Color.White);
```

4.  Hit F5 or click the Run "Local Machine" button.

You should see the text in the top left corner of the screen. Note the order of the batch items is important. If we had put the call to DrawString above the line which draws the texture it would not be show. This is because the texture would be drawn after the text. Because the texture takes up the entire screen it will overwrite the text.  

<a name="Ex1Task7" />
#### Task 7 - Playing Music ####

No game would be complete without Sound Effects and Music. MonoGame has two classes which make working with these items easier. SoundEffect and Song. SoundEffect is usually used to play short audio items, Song is used to play longer items that might need to be streamed. Loading both of these items is done in the same way we load textures and fonts.

Depending on the format of your audio will depend on the defalt content *Processor* that is used to optimize the content. For .wav files the Sound Effect processor is used, for files like .wma the Song processor is used. If you have a .wav file that you want to use for music you can always change the Processor it used in the Pipeline Tool to be "Song - MonoGame"

Lets load the music for the game.

1.  Go back to where we declared the background Texture2D and add
```csharp
	Song song;
```	

2.  Right click on the "Song" class and use the Quick Actions to add a using for *Microsoft.Xna.Framework.Media* or add it manually
```csharp
	using Microsoft.Xna.Framework.Media;
```	

3.  Go to the LoadContent method and add the following underneath were we loaded the texture.
 ```csharp
	song = Content.Load<Song>("sfx/theme");
	MediaPlayer.IsRepeating = true;
	MediaPlayer.Play(song);
```

3. Hit F5 or click the Run "Local Machine" button.

You should hear the music playing when the app starts.

<a name="Ex1Task8" />
#### Task 8 - Playing Sound ####

1.  Go back to where we declared the background Texture2D and add
	SoundEffect playerShot;
2.  Right click on the "SoundEffect" class and use the Quick Actions to add a using for *Microsoft.Xna.Framework.Audio* or add it manually
```csharp
	using Microsoft.Xna.Framework.Audio;
```	
3.  Go to the LoadContent method and add the following underneath were we loaded the texture.
 ```csharp
	playerShot = Content.Load<SoundEffect>("sfx/playerShot");
	playerShot.Play ();
```	
3. Hit F5 or click the Run "Local Machine" button.
 
When the application starts you should hear the playerShot sound effect play. 

<a name="Ex1Task9" />
#### Task 9 - DrawableGameComponent ####

At this point you know how to load the basic assets needed to write a game. There are other assets like Shaders and custom content but to get started all you need is Textures, Fonts, SoundEffects and Music. We can now start writing the game. Our game will have a couple of screens. A Title screen and a Game screen. Each will be responsible for rendering itself and responding to player input. To implement these we are going to use the *DrawableGameComponent* class. MonoGame has a concept of *Components* which you can add to the game  and will automatically update and render themselve. First there is athe GameComponent which can be used for code that just needs to update every frame. The DrawableGameComponent adds a virtual Draw method which can be used to draw items. Since both our Title screne and Game screen will need to draw graphics they will both derive from DrawableGameComponent.

1. In the solution explorer right click on the AlienAttackUniveral project and click Add->New Folder. Name this folder Screens.
2. Right click on the Screens folder and click Add->Class. Call this class TitleScreen.cs
3. Add the following using clauses to the top of the TitleScreen.cs . These will import the required MonoGame namespaces
```csharp
	using Microsoft.Xna.Framework;
	using Microsoft.Xna.Framework.Graphics;
```

4. Change the TitleScreen class so it derives from DrawableGameComponent.
```csharp
	class TitleScreen : DrawableGameComponent
```

5. Next add a contructor. The base GameComponent class *requires* a Game instance parameter for its constructor to our constructor needs to look like the following.
```csharp
	public TitleScreen(Game game) : base(game)
        {
	}
```

6. Next we need to add some private fields to hold Textures, Fonts and a SpriteBatch with which to draw them.
```csharp
	private readonly Texture2D _titleScreen;
        private readonly SpriteFont _font;
        private readonly SpriteBatch _spriteBatch;
```

7. Now update the constructor to create/load these items.
```csharp
	public TitleScreen(Game game) : base(game)
        {
            _spriteBatch = new SpriteBatch(game.GraphicsDevice);
            _titleScreen = game.Content.Load<Texture2D>("gfx\\titleScreen");
            _font = game.Content.Load<SpriteFont>("Font");
        }
```

8. Finally we need to override the Draw method. The Draw (and Update) method takes a GameTime parameter. This is how we keep tack of time in the game. It holds how much time has elapsed since the game started and the elasped time since the last frame. we'll use this information later in our GameScreen. For now the Draw method on the TitleScreen just needs to render the texture we loaded as some text.
```csharp
	public override void Draw(GameTime gameTime)
        {
            _spriteBatch.Begin();
            _spriteBatch.Draw(_titleScreen, Vector2.Zero, Color.White);
            _spriteBatch.DrawString(_font, "Press Enter or Tap to Play", new Vector2(1200, 960), Color.White);
            _spriteBatch.End();
        }
```

In the next section we will update the AlienAttackGame class to make use of this new class. 

<a name="Ex1Task10" />
#### Task 10 - Updating the Game class ####

Now that we have a basic TitleScreen we need to be able to render it. To do that we need to remove all of the code we added to the AlienAttackGame class earlier and add a few new methods.

1. Open the AlienAttackGame.cs if it is not already open.
2. Add a using clause for 
```csharp
	using AlienAttackUniversal.Screens;
```	
3. Select the *entire* AlienAttackGame *class* and replace it with the following 
```csharp
	/// <summary>
	/// This is the main type for your game
	/// </summary>
	public class AlienAttackGame : Game
	{        
	        public AlienAttackGame()
	        {            
	        }
	
	        /// <summary>
	        /// Allows the game to perform any initialization it needs to before starting to run.
	        /// This is where it can query for any required services and load any non-graphic
	        /// related content.  Calling base.Initialize will enumerate through any components
	        /// and initialize them as well.
	        /// </summary>
	        protected override void Initialize()
	        {
	            base.Initialize();
	        }
	
	        /// <summary>
	        /// LoadContent will be called once per game and is the place to load
	        /// all of your content.
	        /// </summary>
	        protected override void LoadContent()
	        {
	        }
	
	        /// <summary>
	        /// UnloadContent will be called once per game and is the place to unload
	        /// all content.
	        /// </summary>
	        protected override void UnloadContent()
	        {
	        }
	
	        /// <summary>
	        /// Allows the game to run logic such as updating the world,
	        /// checking for collisions, gathering input, and playing audio.
	        /// </summary>
	        /// <param name="gameTime">Provides a snapshot of timing values.</param>
	        protected override void Update(GameTime gameTime)
	        {
	            base.Update(gameTime);
	        }
	
	        /// <summary>
	        /// This is called when the game should draw itself.
	        /// </summary>
	        /// <param name="gameTime">Provides a snapshot of timing values.</param>
	        protected override void Draw(GameTime gameTime)
	        {
	        }        
	}
```

4. We need to create an enumeration to define the current *screen* in the game. Add the following code above the AlienAttackGame class.	
```csharp
 	public enum GameState
	{
		TitleScreen,
		GameScreen
	};
```
5. Now we need to declare a few properties to store which screen we are on and allow access to the instance of the game from anywhere. We also want to define our screen resolution that we want to use. Add the following to the top of the AlienAttackGame class
```csharp
	public static AlienAttackGame Instance;
	private readonly GraphicsDeviceManager _graphics;
	private DrawableGameComponent _screen;
	public static int ScreenWidth = 1920;
	public static int ScreenHeight = 1080;
```

6. Next update the constructor to create/set all of these properties
```csharp
	Instance = this;
	_graphics = new GraphicsDeviceManager(this);
	// set our screen size based on the device
	_graphics.PreferredBackBufferWidth = ScreenWidth;
	_graphics.PreferredBackBufferHeight = ScreenHeight;
	Content.RootDirectory = "Content";
```

Note we set the static Instance field to this. Using this field we can access the *instance* of the game just by using AlienAttackGame.Instance. We also create our graphics device manager and set our preferred resolution. The Content manager is also given the "RootDirectory" which is the base directory it will look for the compiled assets.

7. Next we need to add a method which will allow us to change which screen we are showing. This methid is called SetState and will update the _screen field we just created. Add the following code somewhere near the bottom of the AlienAttackGame class.
```csharp
	public void SetState(GameState newState)
	{
		switch(newState)
		{
			case GameState.TitleScreen:
				_screen = new TitleScreen(this);
				break;
			case GameState.GameScreen:
				break;
		}
	}
```	
This method currently only updates the _screen field for a TitleScreen gamestate. We'll add code for the GameScreen later.

8. Now find the Initialize method and add a call to SetState (GameState.TitleScreen). This is to that when the game starts it will default to the title screen. Initialize is only called when the game starts.
```csharp
	protected override void Initialize()
        {
            SetState(GameState.TitleScreen);
            base.Initialize();
        }
```

9. Add the following code to the Update method. This will make sure that our *current* screen is updated every frame.
```csharp
	_screen.Update(gameTime);
```	

10. Finally add the following code to the Draw methid. This will draw our *current* screen.
 ```csharp
 	_screen.Draw(gameTime);
```

11. Hit F5 or click the Run "Local Machine" button.

You should now see the title screen rendered.

<a name="Ex1Task11" />
#### Task 11 - RenderTargets ####

We managed to get our title screen rendering in the last Task. But it wasn't scaling to fit the screen? If you resized the window it didn't stretch to fill it. What is going on?

Well spriteBatch is a low level drawing system. It doesn't care about scaling or fitting graphics to a window. So we need to somehow scale our graphics ourselves to atapt to the various screen resolutions devices can have. Remember Windows 10 supports everything from a Phone right up to a Xbox One with a 4K TV on it. We need a way to handle ALL of those devices. 

SpriteBatch can take a scale matix which it can use to scale the coordinates and graphics passed to it. But this method can be a bit msessy. Wouldn't it be great if there was a way to draw at a fixed known resolution and then scale that? Well it turns out there is its called a RenderTarget. Simply put a RenderTarget is a texture to can draw to. When to tell the graphics device to use a RenderTarget you are basically telling it to not draw on the screen but onto the texture. Now because a RenderTarget derives from Texture we can then use it with a spriteBatch to draw but scale the results. This way we can draw our game at a fixed resolution. 

What we will do next is add a new helper class called *RenderTargetScaler* which we can then use to scale our entire game to fit the window.

1. Right click on the AlienAttackUniversal Project in the solution explorer and click Add->Class and add a "RenderTargetScaler.cs"
2. Add the following using clauses to the top of RenderTargetScaler.cs to bring in the required namespaces.
```csharp
	using Microsoft.Xna.Framework;
	using Microsoft.Xna.Framework.Graphics;
```	
3. Next we need a few fields for our scaler. The most important of which is our _drawBuffer which is a RenderTarget2D. This is what we will draw our entire game to before using the scaler to draw the _drawBuffer on the screen.
```csharp
	private readonly RenderTarget2D _drawBuffer;
        private readonly GraphicsDeviceManager _graphicsDeviceManager;
        private readonly int _screenWidth;
        private readonly int _screenHeight;
        private readonly Game _game;
        private readonly SpriteBatch _spriteBatch;
```

Note there is another SpriteBatch in this list. Normally you will want to keep the number of SpriteBatch instances to a minimum, this is because they maintain an internal list of items that need to be rendered. Its more memory efficient to only have one or two of them. 
4. Next we need to add a constructor. This will need to take an instance of the Game class so we can get things like the Window, the GraphihcsDeviceManager we created in the AlienAttackGame class and the resolution we are going to draw our game in.

```csharp
	public RenderTargetScaler(Game game, GraphicsDeviceManager graphicsDeviceManager, int width, int height)
        {
        }
```

5. Next we set all of our local fields and create an instance of the _spriteBatch in the new constructor we just added.
```csharp
	_game = game;            
	_graphicsDeviceManager = graphicsDeviceManager;
	_screenWidth = width;
	_screenHeight = height;
	_spriteBatch = new SpriteBatch(graphicsDeviceManager.GraphicsDevice); 
```

6. We now need to create our render target. This requires the GraphicsDevice, and our target width and height.
```csharp
	_drawBuffer = new RenderTarget2D(graphicsDeviceManager.GraphicsDevice, width, height);
```

7. That is all we need in the constructor. Next we need a way to tell the GraphicsDevice to use the render target not the screen. We can do that via the GraphicsDevice.SetRenderTarget method. This method takes a RenderTarget2D instance or a null, if we pass null that tells the GraphicsDevice to draw to the screen. If we pass an instance it will use the RenderTarget. We'll add a helper methd to make things a bit eaier for ourselves
```csharp
	public void SetRenderTarget()
	{
		_graphicsDeviceManager.GraphicsDevice.SetRenderTarget(_drawBuffer);
	}
```

Any spriteBatch draw calls made after this method is called will draw on the _drawBuffer. As soon as we call SetRenderTartet (null) it will revert back to drawing on the screen. We'll add that in our next helper method.

8. Now we need a Draw method. This is where we will calculate our scaling factor, clear the render target (using null), then use the spriteBatch to draw our _drawBuffer to the screen. For now just add the new Draw method
```csharp
	public void Draw ()
	{
	}
```

In the next section we'll figure out our scaling factors and fill in the rest of the Draw method.

<a name="Ex1Task12" />
#### Task 12 - Scaling to Screen ####

In the previous section we added a helper class to handle scaling our game. When scaling we will want to use a techique called Letterboxing. This is where rather than just stretching the texture so it fills the entire window, we stretch it so we maintain the aspect ration of the texture. For example of we have a texture that is 1024x768 (4:3) and stretched it to a window that was 1280x720 (16:9) that would look really really weird and distorted. So what we do it create a "letterbox" by adding padding to either the top/bottom or the left/right to make sure we maintain the origional aspect ratio of the texture. 

1. You should already be in the Draw method. The first thing we need to do is figure out the current aspect ratios for our window and our fixed resolution we are drawing at. We do that by dividing the width by the height.
```csharp
	float outputAspect = _game.Window.ClientBounds.Width / (float)_game.Window.ClientBounds.Height;
	float preferredAspect = _screenWidth / (float)_screenHeight;
```

2. Add a Rectangle called dst. This will hold our destination rectangle which is where we will draw the _drawBuffer. We will calcuate this in the next step.
```csharp
	Rectangle dst;
```

3. Next we need to decide if we need to add padding to the top or the sides to get our letterbox effect. Of our *outputAspect* is less than the *preferredAspect* we need to pad the top and bottom. Otherwise we need to add the padding to the left and right.
```csharp
	if (outputAspect <= preferredAspect)
	{
		// output is taller than it is wider, bars on top/bottom
		int presentHeight = (int)((_game.Window.ClientBounds.Width / preferredAspect) + 0.5f);
		int barHeight = (_game.Window.ClientBounds.Height - presentHeight) / 2;
		dst = new Rectangle(0, barHeight, _game.Window.ClientBounds.Width, presentHeight);
	}
	else
	{
		// output is wider than it is tall, bars left/right
		int presentWidth = (int)((_game.Window.ClientBounds.Height * preferredAspect) + 0.5f);
		int barWidth = (_game.Window.ClientBounds.Width - presentWidth) / 2;
		dst = new Rectangle(barWidth, 0, presentWidth, _game.Window.ClientBounds.Height);
	}
```

4. Now we have the dst Rectangle calulcated we can start drawing the _drawBuffer. But before we do we need to make sure the GraphicsDevce is NOT using the render target.
```csharp
	_graphicsDeviceManager.GraphicsDevice.SetRenderTarget(null);
```	

5. Next we want to clear the screen with a nice Black color. You could use a different color but black is normally used because it matches the default color of a screen :).
```csharp
	_graphicsDeviceManager.GraphicsDevice.Clear(Color.Black);
```

6. Finally we use the spriteBatch to draw our _drawBuffer. Note this time we are using the dst Rectangle rather than a Vector2 to define where on the screen this texture will be drawn. The SpriteBatch will scale the texture to fill that Rectangle, and because we took care to calculate our Rectangle based on the aspect ratio it will look correct.
```csharp
	_spriteBatch.Begin();
	_spriteBatch.Draw(_drawBuffer, dst, Color.White);
	_spriteBatch.End();
```

7. We now have all the bits in place in our helper class. Now go back to the AlienAttackGame class and add field for the RenderTargetScaler below all the other fields
```csharp
	private RenderTargetScaler _scaler;
```

8. In the AlienAttackGame Initialize method create an instance of the _scaler just BEFORE we call SetGameState
```csharp
	_scaler = new RenderTargetScaler(this, _graphics, ScreenWidth, ScreenHeight);
```

9. Finally update the Draw method of the AlienAttackGame to match the following code 
```csharp
	protected override void Draw(GameTime gameTime)
	{
		_scaler.SetRenderTarget();
		_screen.Draw(gameTime);
		_scaler.Draw();
	}
```

Note how we call _scaler.SetRenderTarget () first then after we draw the screen , we then call _scaler.Draw. This will make sure the screen is correctly scaled for the window.

<a name="Ex1Task13" />
#### Task 13 - Handling Input ####

In this task we will add input support for our game. MonoGame comes with a complete set of classes for getting input such as GamePads, Keyboard, Mouse and TouchScreens. All of these impliment a similar *state* based api. This allows you to get the current state of the input decice. Because games are generaelly not event driven we will be calling these *GetState* methods every frame. 

Rather than littering our game code with lots of different input methods we will wrap them all up into on InputManager class. This will have one Update method to retrive the current input state. It will then expose this state via some static variables so we can access it throught the game code. 


1.  Right click on the AlienAttackUniversal Project in the solution explorer and click Add->Class and add a "InputManager.cs". Then change the InputManager class to be static.
```csharp
	static class InputManager
	{
	}
```

2.  Add the following using clauses to the top of the InputManager.cs class file
```csharp
	using Microsoft.Xna.Framework;
	using Microsoft.Xna.Framework.Input;
	using Microsoft.Xna.Framework.Input.Touch;
```	
3. Add the new ControlState enumeation just above the InputManager class.  This structure will be exposed as a static and will let us know which controls are active.
```csharp
	public struct ControlState
	{
		public bool Left;
		public bool Right;
		public bool Start;
		public bool Quit;
		public bool Fire;
		public float FingerPosition;
	}
```

4. We now need to declare some internal fields to hold our control *state*. Note we store the current AND previous state for Keyboard and GamePads. This allows us to caclualate what has changed between frames.
 
```csharp
	private static KeyboardState _keyboardState, _lastKeyboard;
	private static GamePadState _gamePadState, _lastGamePad;
	private static ControlState _controlState;
```

5. Next we need a static constructor. In this we will tell the TouchPanel class which *Gestures* we can enabled. In this case if a user taps the screen or id they drag horizontally. We will use the former to handle firing and the latter for moving the player.
 
```csharp
	static InputManager()
	{
		TouchPanel.EnabledGestures = GestureType.Tap | GestureType.HorizontalDrag;
	}
```

5. Its time to add the update method. This needs to be public and static so it can be called from anywhere in the game.
```csharp
	public static void Update()
	{
	}
```

6. Before we fill in the Update method we will add the public static property for the ControlState structure we decalred earlier.
```csharp
	public static ControlState ControlState
	{
		get { return _controlState; }
	}
```

7. Now for the intersting bit. We need to retireve the current state for the all the inputs then caclulate the ControlState. First this to do is to get the Keyboard and GamePad states. Add the following code to the Update method.
```csharp
	_keyboardState = Keyboard.GetState();
	_gamePadState = GamePad.GetState(PlayerIndex.One);
```

8. Now we need to set the ControlState fields depending on the states we got. For example the following code 
```csharp
	_controlState.Quit = (_gamePadState.Buttons.Back== ButtonState.Pressed || _keyboardState.IsKeyDown(Keys.Escape));
```	
  sets the "Quit" field if the Gamepad "Back" buttons is pressed OR is the Escape Key is pressed on the keyboard. 
  Add the following code to the Update method.

```csharp
	_controlState.Quit = (_gamePadState.Buttons.Back == ButtonState.Pressed || _keyboardState.IsKeyDown(Keys.Escape));
        _controlState.Start = (_gamePadState.Buttons.B == ButtonState.Pressed || _keyboardState.IsKeyDown(Keys.Enter) || 		_keyboardState.IsKeyDown(Keys.Space) || _gamePadState.IsButtonDown(Buttons.Start));
         _controlState.Left = (_gamePadState.DPad.Left == ButtonState.Pressed || _gamePadState.ThumbSticks.Left.X < -0.1f || _keyboardState.IsKeyDown(Keys.Left));
        _controlState.Right = (_gamePadState.DPad.Right == ButtonState.Pressed || _gamePadState.ThumbSticks.Left.X > 0.1f || _keyboardState.IsKeyDown(Keys.Right));
        _controlState.Fire = ((_gamePadState.Buttons.B == ButtonState.Pressed && _lastGamePad.Buttons.B == ButtonState.Released) || (_keyboardState.IsKeyDown(Keys.Space) && !_lastKeyboard.IsKeyDown(Keys.Space)));
```        
        
9. That covers the GamePad and Keyboard. Next is the Touch Gestures. The TouchPanel class exposed s IsGestureAvailable property which tells us if a gesture was detected. It also provides a ReadGesture method to read the current gesture.  Note Gestures are queued so it is possible to get multiple gestures in on frame.. e.g a Tap and a Drag. So we need to loop through until IsGestureAvailable is false. Add the following code to the Update Method
 
```csharp
	while(TouchPanel.IsGestureAvailable)
	{
		GestureSample gs = TouchPanel.ReadGesture();
		_controlState.Fire |= (gs.GestureType == GestureType.Tap);
		_controlState.Start |= (gs.GestureType == GestureType.Tap);
		if(gs.GestureType == GestureType.HorizontalDrag)
		{
			_controlState.Left = (gs.Delta.X < 0);
			_controlState.Right = (gs.Delta.X > 0);
			_controlState.FingerPosition = gs.Position.X;
		}
	}
```

You shold be able to see we just loop while we have gestures, and depending on the gesture type we will set the control state. If we get a horizontal drag we will set the Left/Right control state fields depending on which direction the drag was it. We also store the current position for later.

10. Finally we need to store the current state in the _last*State fields so we can detect changes in the next frame.

```csharp
	_lastGamePad = _gamePadState;
        _lastKeyboard = _keyboardState;
```

That covers the InputMananger. We can now use code like the following anywhere in the game to check the input.

```csharp
	If (InputManager.Controlstate.Quit) {
		// quit the game
	}
```

Our next task is the Audio Manager.

<a name="Ex1Task14" />
#### Task 14 - Audio Manager ####

Just like the InputManager the audio manager will be responsible for handling the loading and playing of all audio in the game. 

1.  Right click on the AlienAttackUniversal Project in the solution explorer and click Add->Class and add a "AudioManager.cs". Then change the AudioManager class to be static.
```csharp
	static class AudioManager
	{
	}
```	
2.  Add the following using clauses to the top of the AudioManager.cs file
```csharp
	using Microsoft.Xna.Framework.Audio;
	using Microsoft.Xna.Framework.Media;
```	
3. Add the following Cue enumeration to the AudioManager class. This will let us tell the Manager which sound effect we want to play
```csharp
	public enum Cue
	{
		EnemyShot,
		PlayerShot,
		Explosion
	};
```

4. Now we need to add the fields which we will store the sounds in. We will use Song for the music and SoundEffect for each of the game sounds we want to play. 
```csharp
	private static readonly Song _theme;
        private static readonly SoundEffect _enemyShot;
        private static readonly SoundEffect _playerShot;
        private static readonly SoundEffect _explosion;
```

5. Next we will add a static constructor to load all the audio from the ContentManger. We'll use the Instance field of the Game to get access to the game instance then call the Content.Load methods for each sound.
```csharp
	static AudioManager()
	{
		_theme = AlienAttackGame.Instance.Content.Load<Song>("sfx\\theme");           
		_enemyShot = AlienAttackGame.Instance.Content.Load<SoundEffect>("sfx\\enemyShot");
		_playerShot = AlienAttackGame.Instance.Content.Load<SoundEffect>("sfx\\playerShot");
		_explosion = AlienAttackGame.Instance.Content.Load<SoundEffect>("sfx\\explosion");
	}
```

6. We now need a way to play the sounds. This is where the Cue enumeration comes in. We'll add a new method "PlayCue" which will take the Cue enum as a parameter. We can then use the swtich statement to play the correct sound.
```csharp
	public static void PlayCue(Cue cue)
	{
		// play the effect requested
		switch(cue)
		{
			case Cue.EnemyShot:
				_enemyShot.Play();
				break;
			case Cue.PlayerShot:
				_playerShot.Play();
				break;
			case Cue.Explosion:
				_explosion.Play();
				break;
		}
	}
```

7. Finally we need a couple of methods to start and stop the theme music. 
```csharp
	public static void StartTheme()
        {
            MediaPlayer.IsRepeating = true;
            MediaPlayer.Play(_theme);
        }
        public static void StopTheme()
        {
            MediaPlayer.Stop();
        }
```

This is it for the AudioManager. 

<a name="Ex1Task15" />
#### Task 15 - Sprite ####

We now have allot of the basic plumbing in place we can start looking a implementing our players and enemies for the game. We will want to have animated items or sprite because games without animation are kinda dull. Rather than duplicating a bunch of code for enemies and the player we will create a base class which will have most of that logic. There are excellent third party sprite animation systems out there which make this job allot easier. But for this Lab we'll only use what is available in MonoGame.

To animate our sprites we will use a array of Texture2D's to hold the Texture for each frame. Then just increment an index to change which frame is show. Once we get to the end of the array we will rest the index back to 0 and start again. This is an extremely simple way of animating sprites.

1. In the solution explorer right click on the AlienAttackUniveral project and click Add->New Folder. Name this folder Sprites.
2. Right click on the Sprites folder and click Add->Class. Call this class Sprite.cs 
3. Add the following using clauses
```csharp
	using Microsoft.Xna.Framework;
	using Microsoft.Xna.Framework.Content;
	using Microsoft.Xna.Framework.Graphics;
```

4. First we'll add the properties we need. The first section will hold the information we need for holding and indexing the animations
```csharp
	// all textures in animation set
	protected Texture2D[] Frames { get; set; }
	// current frame to draw
	protected int FrameIndex { get; set; }
	// total number of frames
	protected int FrameCount { get; set; }
```

5. Next we'll add some code which defines the position, size, velocity and Scale of the sprite. 
```csharp
	// size of sprite
	public Vector2 Size { get { return new Vector2(Width, Height); } }
	public int Width { get; set; }
	public int Height { get; set; }
	public Vector2 Position { get; set; }
	public Vector2 Velocity { get; set; }
	public float Rotation { get; set; }
	public Vector2 Scale { get; set; }
```

6. Now we add a few additonal fields for keeping track of the elasped time for animating and the bounding box for handling collisions. The _animationDirection field will be used to left us reverse the direction of the animation.
```csharp
	// variable to track number of millieconds for animations
	private double _time;
	// bounding box of sprite...used for collisions
	private Rectangle _boundingBox;
	private int _animationDirection = 1;
```

7. We need a constructor to set the initial value for the Scale
```csharp
	public Sprite()
	{
		Scale = Vector2.One;
	}
```

8. We need to be able to load textures for our sprite. Our animated graphics are stored on disk in the format	
 		<name>_<frame>
   So we need two LoadContent methods. One to load load a normal non animated sprite and one which will load a group of textures into the array.
```csharp
	public virtual void LoadContent(ContentManager contentManager, string name)
	{
		// load single frame
		Frames = new Texture2D[1];
		Frames[0] = contentManager.Load<Texture2D>(name);
		Width = Frames[0].Width;
		Height = Frames[0].Height;
	}

	public virtual void LoadContent(ContentManager contentManager, string name, int count)
	{
		// load multiple frames
		Frames = new Texture2D[count];
		for(int i = 0; i < count; i++)
			Frames[i] = contentManager.Load<Texture2D>(string.Format(name, i));
		FrameCount = count;
		Width = Frames[0].Width;
		Height = Frames[0].Height;
	}
```

9. Next is a couple of methods we will use to animate the sprite. These will be called as part of the Update method and will take a GameTime so we can accurately track how much time has passed. It will also take a value defining how long a *frame* should last before switching to the next one.

```csharp
	public virtual void AnimateLoop(GameTime gameTime, double frameTime)
	{
		// count number of milliseconds
		_time += gameTime.ElapsedGameTime.TotalMilliseconds;
		// if we're over the time for the next frame, move on
		if(_time > frameTime)
		{
			FrameIndex++;
			_time -= frameTime;
		}
		// if we're past the # of frames, start back at 0
		if(FrameIndex > FrameCount-1)
			FrameIndex = 0;
	}

	public virtual void AnimateReverse(GameTime gameTime, double frameTime)
	{
		// same as above, but reverse direction instead of starting back at 0
		_time += gameTime.ElapsedGameTime.TotalMilliseconds;
		if(_time > frameTime)
		{
			_time -= frameTime;
			if(FrameIndex == 0)
				_animationDirection = 1;
			else if(FrameIndex == FrameCount-1)
				_animationDirection = -1;
			FrameIndex += _animationDirection;
		}
	}
```

10. Next up is the Update method. Again this will take a GameTime so we can move the player depending on how much time has elapsed since the last frame. If we just incrememted the position by Velocity each frame then the game would run faster on a faster PC than it does on a slower one. This is becaue the faster PC will be calling Update/Draw more often. This is one of the most important things to remember when writing games. You frame rate is not always going to be the same on everymachine.

```csharp
	public virtual void Update(GameTime gameTime)
	{
		// move the sprite 1 velocity unit
		Position += Velocity * gameTime.ElapsedGameTime.Milliseconds;
	}
```

11. The draw method is really simple. We just use a passed in spritebatch to draw the current frame. If we don't have any frames we just exit out of the method.
	
```csharp
	public virtual void Draw(GameTime gameTime, SpriteBatch spriteBatch)
	{
		if(Frames == null)
			return;
		spriteBatch.Draw(Frames[FrameIndex], position:Position, color:Color.White, scale:Scale);
	}
```

12. Finally we need to expose the BoundingBox. Remembering that as the sprite moves location on the screen its bounding box location will change.
	
```csharp
	public virtual Rectangle BoundingBox
	{
		get 
		{
			// only need to assign this once
			if(_boundingBox == Rectangle.Empty)
			{
				_boundingBox.Width = Width;
				_boundingBox.Height = Height;
			}
			_boundingBox.X = (int)Position.X;
			_boundingBox.Y = (int)Position.Y;
				return _boundingBox;
		}
	}
```

That is our sprite base class complete. We can now start adding our Player.

<a name="Ex1Task16" />
#### Task 16 - Adding the Player ####

Because we now have a base class for our sprites adding new sprites is going to be very straighforward.

1. Right click on the Sprites folder and click Add->Class. Call this class Player.cs 
2. Change the Player so it derives from Sprite
```csharp
	class Player : Sprite	
	{
	}
```

3. We now need to load the textures for this spirte. So lets just add a constructor and call LoadContent in it.
```csharp
	public Player()
	{
		LoadContent(AlienAttackGame.Instance.Content,  "gfx\\player\\player");
	}
```

That is it! All the other logic is handled in the Sprite class. 

<a name="Ex1Task17" />
#### Task 17 - Game Screen ####

We already added a TitleScreen to our project which derived from DrawableGameComponent. We need to do the same for the GameScreen

1. Right click on the Screens folder and click Add->Class. Call this class GameScreen.cs
3. Add the following using clauses to the top of the TitleScreen.cs . These will import the required MonoGame namespaces
```csharp
	using System.Collections.Generic;
	using Microsoft.Xna.Framework;
	using Microsoft.Xna.Framework.Graphics;
```	
4. Change the GameScreen class so it derives from DrawableGameComponent.
```csharp
	class GameScreen : DrawableGameComponent
```	

5. Add the following fields. We need a SpriteBatch to draw our graphics and field to hold our Player sprite.

```csharp
	private Player _player;
        private readonly SpriteBatch _spriteBatch;
```

6. Add a constructor for the GameScreen like we did with the TitleScreen. Remember it needs the game class as a paremeter. In the constructor we willl create the spriteBatch, the player and start playing the music with the AudioManager.
```csharp
	public GameScreen(Game game) : base(game)
        {
        	_spriteBatch = new SpriteBatch(game.GraphicsDevice);
        	AudioManager.StartTheme();
        	_player = new Player();
		_player.Position = new Vector2(AlienAttackGame.ScreenWidth / 2 - _player.Width / 2, AlienAttackGame.ScreenHeight - 120);
        }
```

7. Next Up is a method to move the player.  We will call this from the GameScreen Update method. We will set our default player velocity to zero so we don't move.. Then if we get any input we will change the Velocty so the player goes in the direction we want. We then call _player.Update to apply the Position += Velocity logic we added to the Sprite class.
 
```csharp
	private void MovePlayer(GameTime gameTime)
        {
            if (_player != null)
            {
                _player.Velocity = Vector2.Zero;
                // move left
                if (InputManager.ControlState.Left && _player.Position.X > 0)
                    _player.Velocity = new Vector2(-400 / 1000.0f, 0);
                // move right
                if (InputManager.ControlState.Right && _player.Position.X + _player.Width < AlienAttackGame.ScreenWidth)
                    _player.Velocity = new Vector2(400 / 1000.0f, 0);
                _player.Update(gameTime);
            }
        }
```

8. Now add the Update method override which will call MovePlayer
```csharp
	public override void Update(GameTime gameTime)
        {
            MovePlayer(gameTime);
        }
```

9. Now Add our Draw method 
```csharp
	public override void Draw(GameTime gameTime)
        {
            _spriteBatch.Begin();
            // draw the player
            if (_player != null)
                _player.Draw(gameTime, _spriteBatch);
            _spriteBatch.End();
        }
```

10. If you ran the game now you would still be stuck on the title screen. We need to hook up the GameState changes. Open AlienAttackUniversal.cs and find the Update method then add above the call to _screen.Update.
```csharp
	InputManager.Update();
```	
        
11. Go to the SetState method and change it so that we create the GameScreen
 
```csharp
	public void SetState(GameState newState)
        {
            switch (newState)
            {
                case GameState.TitleScreen:
                    _screen = new TitleScreen(this);
                    break;
                case GameState.GameScreen:
                    _screen = new GameScreen(this);
                    break;
            }
        }
```

12. Finally Open up the TitleScreen and add the following Update override method. This will check for input and then call the SetState method.
 
```csharp
	public override void Update(GameTime gameTime)
	{
		if(InputManager.ControlState.Start)
			AlienAttackGame.Instance.SetState(GameState.GameScreen);
	}
```

13. Hit F5 or click the Run "Local Machine" button.
 
You should see your spaceship at the bottom of the screen. And if you press Left/Right arrow it should move. 

<a name="Ex1Task18" />
#### Task 18 - Shooting ####

Our next task is to get our ship to shoot. 

1. Right click on the Sprites folder and click Add->Class. Call this class PlayerShot.cs 
2. Change the PlayerShop so it derives from Sprite
```csharp
	class PlayerShot : Sprite
```

3. Add the following using clause
```csharp
	using Microsoft.Xna.Framework;
```	

4. We now need to load the textures for this spirte. So lets just add a constructor and call LoadContent in it. We also need to set the Velocity for this sprite. Note we are using an animated sprite this time, there are 3 frames availalbe pshot_0, pshot_1 and pshot_2
```csharp
	public PlayerShot()
	{
		LoadContent(AlienAttackGame.Instance.Content, "gfx\\pshot\\pshot_{0}", 3);
		Velocity = new Vector2(0, -300 / 1000.0f);
	}
```		

5. We also need to override the Update method. This will let use update the animation for this sprite.
```csharp
	public override void Update(GameTime gameTime)
	{
		base.Update(gameTime);
		AnimateReverse(gameTime, 100);
	}
```
6. Now go back to the GameScreen.cs. We need to add a list to hold the instances of PlayerShot. We need a list because no one likes a gun that only has one bullet :) We also need a field in which to store the last game time what we fired a bullet. This is so that there is a delay between shots. 
```csharp
	private readonly List<PlayerShot> _playerShots;
	private double _lastTime;
```
7. In the GameScreen constructor create a new instalce of _playerShots
```csharp
	 _playerShots = new List<PlayerShot>();
```
8. Now we add a UpdatePlayerShots method. You can see in the code below that we only fire a bullet every 500 milliseconds. We also make use of the AudioManager to play the "PlayerShot" sound. This method also makes sure we update ALL the shots on the screen and remove the ones that have made it off the top of the screen.
```csharp
	private void UpdatePlayerShots(GameTime gameTime)
        {
            // if we are allowed to fire, add a shot to the list
            if (_player != null && InputManager.ControlState.Fire && gameTime.TotalGameTime.TotalMilliseconds - _lastTime > 500)
            {
                // create a new shot over the ship
                PlayerShot ps = new PlayerShot();
                ps.Position = new Vector2((_player.Position.X + _player.Width / 2.0f) - ps.Width / 2.0f, _player.Position.Y - ps.Height);
                _playerShots.Add(ps);
                _lastTime = gameTime.TotalGameTime.TotalMilliseconds;
                AudioManager.PlayCue(AudioManager.Cue.PlayerShot);
            }

            // enumerate the player shots on the screen
            for (int i = 0; i < _playerShots.Count; i++)
            {
                PlayerShot playerShot = _playerShots[i];

                playerShot.Update(gameTime);

                // if it's off the top of the screen, remove it from the list
                if (playerShot.Position.Y + playerShot.Height < 0)
                    _playerShots.RemoveAt(i);
            }
        }
```

9. We also need to call UpdatePlayerShots in the Update method of GameScreen. Your update method should look lile the following
```csharp
	public override void Update(GameTime gameTime)
        {
            MovePlayer(gameTime);
            UpdatePlayerShots(gameTime);
        }
```

10. Finally we need to draw all of the shots. Add the following to the GameScreen Draw call. Remember order is important. Since the shots would normally come out from underneath the ship we shoild draw the shots first.
```csharp
	foreach(PlayerShot playerShot in _playerShots)
		playerShot.Draw(gameTime, _spriteBatch);
```

11.  Hit F5 or click the Run "Local Machine" button.

Start the game and by hitting Space you should fire the weapon. Now we just need something to shoot at!

<a name="Ex1Task19" />
#### Task 19 - Enemy ####

<a name="Ex1Task20" />
#### Task 20 - Enemy Group ####

<a name="Ex1Task21" />
#### Task 21 - The Explosive Finale ####

<a name="Summary" />
## Summary ##

By completing this module you should have:

- Foo

> **Note:** You can take advantage of the [Visual Studio Dev Essentials]( https://www.visualstudio.com/en-us/products/visual-studio-dev-essentials-vs.aspx) subscription in order to get everything you need to build and deploy your app on any platform.
