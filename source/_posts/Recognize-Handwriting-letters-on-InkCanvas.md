title: Recognize Handwriting Letters on InkCanvas
date: 2015-11-14 22:48:51
categories: [.NET, UWP]
tags: [C#, XAML, InkCanvas]
---
Handwriting letters on <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx">InkCanvas</a> controls can be recognized as follows.

#### XAML
<pre class="brush: xml;">
&lt;InkCanvas x:Name="inkCanvas" />
</pre>

#### Recognition
<pre class="brush: c-sharp;">
namespace MyApp
{
    using Windows.UI.Core;

    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            InitializeComponent();
            inkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Touch | CoreInputDeviceTypes.Pen;
        }        

        private void button_Click(object sender, RoutedEventArgs e)
        {
            ShowRecognizedText();
        }

        private async void ShowRecognizedText()
        {
            var inkRecContainer = new InkRecognizerContainer();
            if (inkCanvas.InkPresenter.StrokeContainer.GetStrokes().Count > 0)
            {
                var result = await inkRecContainer.RecognizeAsync(inkCanvas.InkPresenter.StrokeContainer, InkRecognitionTarget.All);
                if (result != null)
                {
                    var dlg = new Windows.UI.Popups.MessageDialog(result.FirstOrDefault().GetTextCandidates().FirstOrDefault());
                    await dlg.ShowAsync();
                }
            }
        }
    }
}
</pre>

### Result
<img align="left" src="{% post_path Recognize-Handwriting-letters-on-InkCanvas %}jhmt.png" />
<br clear="left">