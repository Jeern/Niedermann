---
title: "Click and double click in XNA Games
"
date: 2010-05-12T02:00:00+02:00
draft: true
aliases:
    - /2010/05/12/ClickAndDoubleClickInXNAGames.aspx
---
Click and double click in XNA Games
Windows developers who are used to click events in WinForms, WPF or Silverlight might miss click and double click events, but because of the game loop where everything is drawn and updated every few milliseconds an event based approach is probably not a good idea in most cases. Not so much because of the performance of an event based approach, but more because the complexity is overwhelming. If several users clicks and double clicks several keys on the keyboard simultaneously, how would you decide which of these are clicked and in which order, and further more it is not obvious to subscribers of the events that they are in the game loop, so they might do code which performs inadequately.

I have made a few generic classes that expands the MouseState, GamePadState and KeyboardState in order to make click “events” available. But of course I utilize the standard polling mechanism in XNA which is to call GetState() on each device.

The approach is that I call my own version of GetState() in the main game loop. My GetState() method first enqueues the state in a Queue, and then dequeues all states older than 500 ms, before the state is returned. In this way a historical map of user interactions is maintained at all times.

Now I can simply check if a key or button was clicked by checking if the key was pressed and then released “historically”. Similarly I can check for double click by checking if the key or button was pressed, then released, then pressed again and then released again.

Now you probably realize why I go back 500 ms. It is because this is the standard time in which a double click should be executed.

I have used this approach in the game Protect the Carrot at http://ptc.codeplex.com and it works like a charm (The url for the game does not work today, since the game will only be published in a few weeks).

Here is the implementation of the MouseExtended class which uses this approach:

```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using Microsoft.Xna.Framework.Input;  
using Microsoft.Xna.Framework;  
  
namespace PTC.Input  
{  
    public class MouseExtended : InputDeviceExtended<MouseState>  
    {  
        private static MouseExtended m_Current;  
        public static MouseExtended Current  
        {  
            get  
            {  
                if (m_Current == null)  
                {  
                    m_Current = new MouseExtended();  
                }  
                return m_Current;  
            }  
        }  
  
        public MouseState GetState(GameTime currentTime)  
        {  
            DequeueOldStates(currentTime);  
            MouseState state = Mouse.GetState();  
            EnqueueNewState(currentTime, state);  
            return state;  
        }  
  
        private bool ClickCount(MouseButton checkButton, int requiredCount)  
        {  
            ButtonState found = ButtonState.Released;  
            int count = 0;  
            foreach (InputStateExtended<MouseState> stateExt in RecordedStates)  
            {  
                if (found == ButtonState.Pressed &&   
                    ButtonStateToCheck(stateExt.State, checkButton) == ButtonState.Released)  
                {  
                    count++;  
                    if (count >= requiredCount)  
                        return true;  
                }  
                found = ButtonStateToCheck(stateExt.State, checkButton);  
            }  
            return false;  
        }  
  
        private ButtonState ButtonStateToCheck(MouseState state, MouseButton checkButton)  
        {  
            switch (checkButton)  
            {  
                case MouseButton.Left:  
                    return state.LeftButton;  
                case MouseButton.Middle:  
                    return state.MiddleButton;  
                case MouseButton.Right:  
                    return state.RightButton;  
                case MouseButton.XButton1:  
                    return state.XButton1;  
                case MouseButton.XButton2:  
                    return state.XButton2;  
                default:  
                    return state.LeftButton;  
            }  
        }  
  
        public bool WasSingleClick(MouseButton checkButton)  
        {  
            return(ClickCount(checkButton, 1));  
        }  
  
        public bool WasDoubleClick(MouseButton checkButton)  
        {  
            return (ClickCount(checkButton, 2));  
        }  
  
    }  
}  
```

And here is the generic InputDeviceExtended class which MouseExtended inherits:

```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using Microsoft.Xna.Framework;  
using Microsoft.Xna.Framework.Audio;  
using Microsoft.Xna.Framework.Content;  
using Microsoft.Xna.Framework.GamerServices;  
using Microsoft.Xna.Framework.Graphics;  
using Microsoft.Xna.Framework.Input;  
using Microsoft.Xna.Framework.Media;  
using Microsoft.Xna.Framework.Net;  
using Microsoft.Xna.Framework.Storage;  
  
namespace PTC.Input  
{  
    public class InputDeviceExtended<S> where S : struct  
    {  
        private Queue<InputStateExtended<S>> m_RecordedStates = new Queue<InputStateExtended<S>>();  
  
        public Queue<InputStateExtended<S>> RecordedStates  
        {  
            get { return m_RecordedStates; }  
        }  
  
        private Stack<InputStateExtended<S>> m_StatesForReuse = new Stack<InputStateExtended<S>>();  
  
        protected void EnqueueNewState(GameTime time, S state)  
        {  
            if (!state.Equals(m_CurrentState))  
            {  
                m_CurrentState = state;  
                m_RecordedStates.Enqueue(CreateState(time, state));  
            }  
        }  
  
        private S m_CurrentState;  
        public S CurrentState  
        {  
            get { return m_CurrentState; }  
        }  
  
        protected void DequeueOldStates(GameTime currentTime)  
        {  
            InputStateExtended<S> state = null;  
            if (m_RecordedStates.Count > 0)  
            {  
                state = m_RecordedStates.Peek();  
            }  
            if (state != null && state.StateTime < currentTime.TotalRealTime.Subtract(new TimeSpan(0, 0, 0, 0, InputDeviceConstants.ClickCountTimeMS)))  
            {  
                m_StatesForReuse.Push(m_RecordedStates.Dequeue());  
                DequeueOldStates(currentTime);  
            }  
        }  
  
        private InputStateExtended<S> CreateState(GameTime time, S state)  
        {  
            if (m_StatesForReuse.Count > 0)  
            {  
                //Reuses the object to fight of the GC  
                InputStateExtended<S> stateExt = m_StatesForReuse.Pop();  
                stateExt.StateTime = time.TotalRealTime;  
                stateExt.State = state;  
                return stateExt;  
            }  
            else  
            {  
                return new InputStateExtended<S>(time, state);  
            }  
        }  
    }  
}  
```

Notice that the Recorded States are reused. When dequeued from the queue the are added to a reuse stack. This is a standard trick to fight of the Garbage Collector, by always keeping a reference to objects on the heap they never become garbage + they are reused so the memory use will not explode.

I have also implemented the necessary extended classes for the Keyboard and the gamepad. I have included them in the attachment to this post. I have not tested the GamepadExtended class since I do not own a Gamepad, but it is implemented exactly as the keyboard and mouse classes and ought to work.

To wire up the new classes you just add the relevant Getstate() calls to the Update game loop like so:

```csharp
protected override void Update(GameTime gameTime)  
{  
    KeyboardExtended.Current.GetState(gameTime);  
    MouseExtended.Current.GetState(gameTime);  
    base.Update(gameTime);  
}  
```

And then you can check for click and double click “events”. For instance you check for double click of the left mouse button like this:

```csharp
if(MouseExtended.Current.WasDoubleClick(MouseButton.Left)) 
{  
    //Do double click reaction  
}  
```

Hope you like the stuff.And look out for the amazing “Protect The Carrot” game within the next month or so, at http://ptc.codeplex.com

In InputDeviceExtended.zip I have included the code for all the InputDevice classes.

[InputDeviceExtended.zip](/resources/ClickAndDoubleClickInXNAGames/InputDeviceExtended.zip)
