# C# 进程间通信

## 使用SendMessage向另一进程发送WM_COPYDATA消息 ##

## Send端： ##

	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Windows;
	using System.Windows.Controls;
	using System.Windows.Data;
	using System.Windows.Documents;
	using System.Windows.Input;
	using System.Windows.Media;
	using System.Windows.Media.Imaging;
	using System.Windows.Navigation;
	using System.Windows.Shapes;
	using System.Runtime.InteropServices;
	
	namespace SentTest
	{
	    /// <summary>
	    /// MainWindow.xaml 的交互逻辑
	    /// </summary>
	    public partial class MainWindow : Window
	    {
	        public MainWindow()
	        {
	            InitializeComponent();
	            this.Title = "发送窗口";
	        }
	
	        const int WM_COPYDATA = 0x004A;
	
	        public struct COPYDATASTRUCT
	        {
	            public IntPtr dwData;
	            public int cData;
	            [MarshalAs(UnmanagedType.LPStr)]
	            public string lpData;
	        }
	
	        [DllImport("User32.dll")]
	        public static extern int SendMessage(int hwnd, int msg, int wParam, ref COPYDATASTRUCT IParam);
	        [DllImport("User32.dll")]
	        public static extern int FindWindow(string lpClassName, string lpWindowName);
	
	        private void button1_Click(object sender, RoutedEventArgs e)
	        {
	            String strSent = "需要发送的信息";
	
	            int WINDOW_HANDLE = FindWindow(null, "接收窗口");
	            if (WINDOW_HANDLE != 0)
	            {
	                byte[] arr = System.Text.Encoding.Default.GetBytes(strSent);
	                int len = arr.Length;
	                COPYDATASTRUCT cdata;
	                cdata.dwData = (IntPtr)100;
	                cdata.lpData = strSent;
	                cdata.cData = len + 1;
	                SendMessage(WINDOW_HANDLE, WM_COPYDATA, 0, ref cdata);
	            }
	        }
	    }
	}

## Get端： ##

	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Windows;
	using System.Windows.Controls;
	using System.Windows.Data;
	using System.Windows.Documents;
	using System.Windows.Input;
	using System.Windows.Media;
	using System.Windows.Media.Imaging;
	using System.Windows.Navigation;
	using System.Windows.Shapes;
	using System.Runtime.InteropServices;
	using System.Windows.Interop;
	
	namespace GetTest
	{
	    /// <summary>
	    /// MainWindow.xaml 的交互逻辑
	    /// </summary>
	    public partial class MainWindow : Window
	    {
	        IntPtr hwnd;
	        public MainWindow()
	        {
	            InitializeComponent();
	            this.Title = "接受窗口";
	
	            this.Loaded += new RoutedEventHandler(MainWindow_Loaded);
	            this.Closed += new EventHandler(MainWindow_Closed);
	        }
	
	        void MainWindow_Loaded(object sender, RoutedEventArgs e)
	        {
	            //窗口加载完毕才可用，否则会报错。
	            hwnd = new WindowInteropHelper(this).Handle;
	            HwndSource source = HwndSource.FromHwnd(hwnd);
	            if (source != null) source.AddHook(WndProc);
	        }
	
	        void MainWindow_Closed(object sender, EventArgs e)
	        {
	            try
	            {
	                HwndSource.FromHwnd(hwnd).RemoveHook(WndProc);
	            }
	            catch (Exception)
	            {
	
	                throw;
	            }
	
	        }
	
	        const int WM_COPYDATA = 0x004A;//WM_COPYDATA消息的主要目的是允许在进程间传递只读数据。
	        //Windows在通过WM_COPYDATA消息传递期间，不提供继承同步方式。
	        //其中,WM_COPYDATA对应的十六进制数为0x004A
	        public struct COPYDATASTRUCT
	        {
	            public IntPtr dwData;
	            public int cData;
	            [MarshalAs(UnmanagedType.LPStr)]
	            public string lpData;
	        }
	
	        //wpf用此方法
	        private IntPtr WndProc(IntPtr hwnd, int msg, IntPtr wParam, IntPtr lParam, ref bool handled)
	        {
	            if (msg == WM_COPYDATA)
	            {
	                COPYDATASTRUCT cdata = new COPYDATASTRUCT();
	                Type mytype = cdata.GetType();
	                cdata = (COPYDATASTRUCT)Marshal.PtrToStructure(lParam, mytype);
	                this.textBox1.Text = cdata.lpData;
	            }
	            return IntPtr.Zero;
	        }
	
			//WinFrom用此方法
	        /* protected override void DefWndProc(ref Message m)
	        {
	            switch (m.Msg)
	            {
	            case WM_COPYDATA:
	                COPYDATASTRUCT cdata = new COPYDATASTRUCT();
	                Type mytype = cdata.GetType();
	                cdata = (COPYDATASTRUCT)m.GetLParam(mytype);
	                this.textBox1.Text = cdata.lpData;
	                break;
	            default:
	                base.DefWndProc(ref m);
	                break;
	            }
	        } */
	    }
	}


# 参考文档： #

[c# 进程间同步实现 进程之间通讯的几种方法 ](http://blog.csdn.net/feiren127/article/details/5459827)
