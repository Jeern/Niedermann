---
title: "API for Sticky Notes in Windows 7
"
date: 2009-11-13T23:28:10.6463905+00:00
draft: true
aliases:
    - /2009/11/13/APIForStickyNotesInWindows7.aspx
---
In the [Day View]({{<ref "/Windows7ContestFinished.md">}}) project I needed to open a Windows 7 Sticky Note programmatically and write to it. Unfortunately I found out that there is not an API for the Sticky Notes application. At least not a managed one.

So I had to fake it and make my own API. I have made a small class that basicly uses Process.Start to open the program, and SendKeys to write to the Note.

Here it is:

```csharp
using System;  
using System.Diagnostics;  
using System.IO;  
using System.Runtime.InteropServices;  
using System.Threading;  
using System.Windows.Forms;  
  
namespace DayView  
{  
    public class StickyNote  
    {  
        private const string m_ProcessName = "StikyNot";  
        private readonly string m_ProcessFileName = Path.Combine(Environment.SystemDirectory, "StikyNot.exe");  
        private event EventHandler m_Activated = delegate { };  
        [DllImport("user32.dll")]  
        [return: MarshalAs(UnmanagedType.Bool)]  
        static extern bool SetForegroundWindow(IntPtr hWnd);  
  
        public void Activate()  
        {  
            bool makeNewNote = true;  
            Process p = FindProcess();  
            if (p == null)  
            {  
                p = StartProcess();  
                if (!NoteContainsText(p.MainWindowHandle))  
                {  
                    makeNewNote = false;  
                }  
            }  
            var state = new StickyNoteState();  
            state.MakeNewNote = makeNewNote;  
            state.StickyNoteProcess = p;  
            ThreadPool.QueueUserWorkItem(Activate, state);  
        }  
  
        private void Activate(object state)  
        {  
            var stickyNoteState = state as StickyNoteState;  
            if (stickyNoteState.MakeNewNote)  
            {  
                NewNote(stickyNoteState.StickyNoteProcess);  
            }  
            OnActivated();  
        }  
  
        private Process StartProcess()  
        {  
            var startInfo = new ProcessStartInfo(m_ProcessFileName);  
            Process p = Process.Start(startInfo);  
            Thread.Sleep(200); //This is an annoying hack. I haven't been able to find another way to be sure the process is started.  
            return p;  
        }  
  
        private void NewNote(Process p)  
        {  
            SetForegroundWindow(p.MainWindowHandle);  
            Signal("^n");              
        }  
  
        /// <summary>  
        /// Weird hack to find out if note contains text.  
        /// </summary>  
        /// <returns></returns>  
        private bool NoteContainsText(IntPtr handle)  
        {  
            string textOfClipboard = Clipboard.GetText();  
            Signal("^a");  
            Signal("^c");  
            Signal("{RIGHT}");  
            string noteText = Clipboard.GetText().Trim();  
            if (textOfClipboard == null)  
            {  
                Clipboard.SetText(textOfClipboard);  
            }  
            return !string.IsNullOrEmpty(noteText);  
        }  
  
        private Process FindProcess()  
        {  
                Process[] processes = Process.GetProcessesByName(m_ProcessName);  
                if(processes != null && processes.Length > 0)  
                {  
                    return processes[0];  
                }  
            return null;  
        }  
  
        internal void OnActivated()  
        {  
            m_Activated(this, new EventArgs());  
        }  
  
        public event EventHandler Activated  
        {  
            add { m_Activated += value; }  
            remove { m_Activated -= value; }  
        }  
  
        public void Signal(string message)  
        {  
            SendKeys.SendWait(message);  
            SendKeys.Flush();  
        }  
    }  
  
    public class StickyNoteState  
    {  
        public bool MakeNewNote { get; set; }  
        public Process StickyNoteProcess { get; set; }  
  
    }  
}  
```

It works OK. The only really buggy thing in the code is in line 47, where I have to use a Thread.Sleep in order to make sure the note is loaded before I write to it. Unfortunately this is necessary because the Process class does not provide an event to signal when it has finished loading. I arbitrarily chose 200 milliseconds for the Sleep. Larger values might be necessary on slower computers.

In order to use the class you have to configure SendKeys in your app.config like this:

```xml
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <appSettings>  
    <add key="SendKeys" value="SendInput"/>  
  </appSettings>  
</configuration>  
```