title: Change Resources Dynamically - Universal Windows Platform (UWP) Apps
date: 2015-11-13 21:35:08
categories: [.NET, UWP]
tags: [C#, XAML, Globalization]
---
<a href="https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged(v=vs.110).aspx">INotifyPropertyChanged</a> interface enables to change 2 or more resource (.resw) files dynamically in Universal Windows Platform (UWP) Apps.

#### Example
Resources.resw
<img align="left" src="{% post_path Change-Resources-Dynamically-UWPApp %}Resources.png" />
<br clear="left">
Resources.de.resw
<img align="left" src="{% post_path Change-Resources-Dynamically-UWPApp %}Resources.de.png" />
<br clear="left">


#### Implement INotifyPropertyChanged
In order to manage resources in one class, implement INotifyPropertyChanged class as like the following 2 classes, ResourceService and ResourceManager.
<pre class="brush: c-sharp;">
namespace MyApp
{
    using System.ComponentModel;
    using System.Runtime.CompilerServices;
    using Windows.ApplicationModel.Resources;

    public class ResourceService : INotifyPropertyChanged
    {
        #region Singleton Members
        private static readonly ResourceService _current = new ResourceService();

        public static ResourceService Current
        {
            get { return _current; }
        }
        #endregion

        #region INotifyPropertyChanged Members
        public event PropertyChangedEventHandler PropertyChanged;
        
        protected virtual void RaisePropertyChanged([CallerMemberName] string propertyName = null)
        {
            if (PropertyChanged != null)
            {
                PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
            }
        }
        #endregion

        readonly ResourceManager _resourceManager = new ResourceManager();

        public ResourceManager ResourceManager
        {
            get { return _resourceManager; }
        }

        public void ChangeCulture(string name)
        {
            ResourceManager.ResourceLoader = ResourceLoader.GetForCurrentView(string.Concat("Resources.", name));
            this.RaisePropertyChanged("ResourceManager");
        }
    }

    public class ResourceManager
    {
        private ResourceLoader _resourceLoader = ResourceLoader.GetForCurrentView();

        public ResourceLoader ResourceLoader
        {
            get { return _resourceLoader; }
            set { _resourceLoader = value; }
        }
        public string HelloWorld
        {
            get { return ResourceLoader.GetString("HelloWorld"); }
        }
    }
}
</pre>


#### Binding 
Bind properties of ResourceManager in XAML file like this:

MainPage.xaml.cs
<pre class="brush: c-sharp;">
public MainPage()
{
	InitializeComponent();
	this.DataContext = ResourceService.Current;
}
</pre>
MainPage.xaml
<pre class="brush: xml;">
&lt;TextBox x:Name="textBox" Text="{Binding Path=ResourceManager.HelloWorld, Mode=OneWay}" />
</pre>

### Switch Hello World!
Now the text can be changed dynamically by calling ResourceService.ChangeCulture method:

Before:
<img align="left" src="{% post_path Change-Resources-Dynamically-UWPApp %}HelloWorld.png" />
<br clear="left">

MainPage.xaml.cs
<pre class="brush: c-sharp;">
private void button_Click(object sender, RoutedEventArgs e)
{
	ResourceService.Current.ChangeCulture("de");
}
</pre>

After:
<img align="left" src="{% post_path Change-Resources-Dynamically-UWPApp %}HalloWelt.png" />
<br clear="left">
