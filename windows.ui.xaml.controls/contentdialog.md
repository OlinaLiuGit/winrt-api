---
-api-id: T:Windows.UI.Xaml.Controls.ContentDialog
-api-type: winrt class
---

<!-- Class syntax.
public class ContentDialog : Windows.UI.Xaml.Controls.ContentControl, Windows.UI.Xaml.Controls.IContentDialog, Windows.UI.Xaml.Controls.IContentDialog2
-->

# Windows.UI.Xaml.Controls.ContentDialog

## -description
Represents a dialog box that can be customized to contain checkboxes, hyperlinks, buttons and any other XAML content.

## -xaml-syntax
```xaml
<ContentDialog .../>
-or-
<ContentDialog>
    singleObject
</ContentDialog>
-or-
<ContentDialog>stringContent</ContentDialog>
```


## -remarks
Use a [ContentDialog](contentdialog.md) to show information in a modal dialog. You can add a [ContentDialog](contentdialog.md) to an app page using code or XAML, or you can create a custom dialog class that's derived from [ContentDialog](contentdialog.md). Both ways are shown in the examples section of this topic.

Use the [Title](contentdialog_title.md) property to put a title on the dialog. To add a complex title element with more than simple text, you can use the [TitleTemplate](contentdialog_titletemplate.md) property.

The [ContentDialog](contentdialog.md) has 2 built-in buttons that let a user respond to the dialog. Use the [PrimaryButtonText](contentdialog_primarybuttontext.md) and [SecondaryButtonText](contentdialog_secondarybuttontext.md) properties to set the display text on each respective button. If you set the text value to an empty string, the button is hidden. Handle the [PrimaryButtonClick](contentdialog_primarybuttonclick.md) and [SecondaryButtonClick](contentdialog_secondarybuttonclick.md) events to get the user's response and do any work before the dialog closes.

To show the dialog, call the [ShowAsync](contentdialog_showasync.md) method. Use the result of this method to determine which of the buttons was clicked.

Only one [ContentDialog](contentdialog.md) can be shown at a time. To chain together more than one [ContentDialog](contentdialog.md), handle the [Closing](contentdialog_closing.md) event of the first [ContentDialog](contentdialog.md). In the [Closing](contentdialog_closing.md) event handler, call [ShowAsync](contentdialog_showasync.md) on the second dialog to show it.

### Control style and template

You can modify the default [Style](../windows.ui.xaml/style.md) and [ControlTemplate](controltemplate.md) to give the control a unique appearance. For information about modifying a control's style and template, see [Styling controls](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/styling-controls). The default style, template, and resources that define the look of the control are included in the generic.xaml file. For design purposes, generic.xaml is available in the \(Program Files)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\&lt;SDK version&gt;\Generic folder from a Windows Software Development Kit (SDK) installation. Styles and resources from different versions of the SDK might have different values.

Starting in Windows 10, version 1607 (Windows Software Development Kit (SDK) version 10.0.14393.0), generic.xaml includes resources that you can use to modify the colors of a control in different visual states without modifying the control template. In apps that target this software development kit (SDK) or later, modifying these resources is preferred to setting properties such as [Background](control_background.md) and [Foreground](control_foreground.md). For more info, see the [Light-weight styling](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/styling-controls) section of the [Styling controls](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/styling-controls) article.

This table shows the resources used by the [ContentDialog](contentdialog.md) control.

<table>
   <tr><th>Resource key</th><th>Description</th></tr>
   <tr><td>ContentDialogForeground</td><td>Text color in dialog</td></tr>
   <tr><td>ContentDialogBackground</td><td>Background color</td></tr>
   <tr><td>ContentDialogBorderBrush</td><td>Border color</td></tr>
</table>

## -examples
This example shows how to create and show a simple [ContentDialog](contentdialog.md) in code.

```csharp
private async void WifiConnectionLost()
{
    ContentDialog noWifiDialog = new ContentDialog()
    {
        Title = "No wifi connection",
        Content = "Check connection and try again",
        PrimaryButtonText = "Ok"
    };

    await noWifiDialog.ShowAsync();
}
```

This example shows how to create a [ContentDialog](contentdialog.md) in the XAML of an app page. Even though the dialog is defined in the app page, it's not shown until you call [ShowAsync](contentdialog_showasync.md) in your code.

Here, the [IsPrimaryButtonEnabled](contentdialog_isprimarybuttonenabled.md) property is set to **false**. The primary button is enabled in code when the user checks the [CheckBox](checkbox.md) to confirm their age.

The [TitleTemplate](contentdialog_titletemplate.md) property is used to create a title that includes both a logo and text.

```xaml
<ContentDialog x:Name="termsOfUseContentDialog" 
           PrimaryButtonText="accept" IsPrimaryButtonEnabled="False"
           SecondaryButtonText="cancel" FullSizeDesired ="True"
           Opened="TermsOfUseContentDialog_Opened">
    <ContentDialog.TitleTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="ms-appx:///Assets/SmallLogo.png" Width="40" Height="40" Margin="10,0"/>
                <TextBlock Text="Terms of use"/>
            </StackPanel>
        </DataTemplate>
    </ContentDialog.TitleTemplate>
    <StackPanel>
        <TextBlock TextWrapping="WrapWholeWords">
        <Run>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor 
             congue massa. Fusce posuere, magna sed pulvinar ultricies, purus lectus 
             malesuada libero, sit amet commodo magna eros quis urna.</Run><LineBreak/>
        <Run>Nunc viverra imperdiet enim. Fusce est. Vivamus a tellus.</Run><LineBreak/>
        <Run>Pellentesque habitant morbi tristique senectus et netus et malesuada fames 
             ac turpis egestas. Proin pharetra nonummy pede. Mauris et orci.</Run><LineBreak/>
        <Run>Suspendisse dui purus, scelerisque at, vulputate vitae, pretium mattis, nunc. 
             Mauris eget neque at sem venenatis eleifend. Ut nonummy.</Run>
        </TextBlock>
        <CheckBox x:Name="ConfirmAgeCheckBox" Content="I am over 13 years of age." 
              Checked="ConfirmAgeCheckBox_Checked" Unchecked="ConfirmAgeCheckBox_Unchecked"/>
    </StackPanel>
</ContentDialog>
```

```csharp
private async void ShowTermsOfUseContentDialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialogResult result = await termsOfUseContentDialog.ShowAsync();
    if (result == ContentDialogResult.Primary)
    {
        // Terms of use were accepted.
    }
    else
    {
        // User pressed Cancel or the back arrow.
        // Terms of use were not accepted.
    }
}

private void TermsOfUseContentDialog_Opened(ContentDialog sender, ContentDialogOpenedEventArgs args)
{
    // Ensure that the check box is unchecked each time the dialog opens.
    ConfirmAgeCheckBox.IsChecked = false;
}

private void ConfirmAgeCheckBox_Checked(object sender, RoutedEventArgs e)
{
    termsOfUseContentDialog.IsPrimaryButtonEnabled = true;
}

private void ConfirmAgeCheckBox_Unchecked(object sender, RoutedEventArgs e)
{
    termsOfUseContentDialog.IsPrimaryButtonEnabled = false;
}
```

This example shows how to create and use a custom dialog (`SignInContentDialog`) derived from [ContentDialog](contentdialog.md).

```xaml

<!-- SignInContentDialog.xaml -->
<ContentDialog
    x:Class="ExampleApp.SignInContentDialog"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:ExampleApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Title="SIGN IN"
    PrimaryButtonText="sign in"  
    SecondaryButtonText="cancel"
    PrimaryButtonClick="ContentDialog_PrimaryButtonClick"
    SecondaryButtonClick="ContentDialog_SecondaryButtonClick">
    
    <ContentDialog.Resources>
    <!-- These flyouts or used for confirmation when the user changes 
         the option to save their user name. -->
        <Flyout x:Key="DiscardNameFlyout" Closed="Flyout_Closed">
            <StackPanel>
                <TextBlock Text="Discard user name?"/>
                <Button Content="Discard" Click="DiscardButton_Click"/>
            </StackPanel>
        </Flyout>
        <Flyout x:Key="SaveNameFlyout" Closed="Flyout_Closed">
            <StackPanel>
                <TextBlock Text="Save user name?"/>
                <Button Content="Save" Click="SaveButton_Click"/>
            </StackPanel>
        </Flyout>
    </ContentDialog.Resources>

    <StackPanel VerticalAlignment="Stretch" HorizontalAlignment="Stretch">
        <TextBox Name="userNameTextBox" Header="User name"/>
        <PasswordBox Name="passwordTextBox" Header="Password" IsPasswordRevealButtonEnabled="True"/>
        <CheckBox Name="saveUserNameCheckBox" Content="Save user name"/>

        <TextBlock x:Name="errorTextBlock" Style="{StaticResource ControlContextualInfoTextBlockStyle}"/>

        <!-- Content body -->
        <TextBlock Name="body" Style="{StaticResource MessageDialogContentStyle}" TextWrapping="Wrap">
            <TextBlock.Text>
                Lorem ipsum dolor sit amet, consectetur adipisicing elit,
                    sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
            </TextBlock.Text>
        </TextBlock>
    </StackPanel>
</ContentDialog>
```

```csharp

// SignInContentDialog.xaml.cs
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;

namespace ExampleApp
{
    public enum SignInResult
    {
        SignInOK,
        SignInFail,
        SignInCancel,
        Nothing
    }

    public sealed partial class SignInContentDialog : ContentDialog
    {
        public SignInResult Result { get; private set; }

        public SignInContentDialog()
        {
            this.InitializeComponent();
            this.Opened += SignInContentDialog_Opened;
            this.Closing += SignInContentDialog_Closing;
        }
        
        private void ContentDialog_PrimaryButtonClick(ContentDialog sender, ContentDialogButtonClickEventArgs args)
        {
            // Ensure the user name and password fields aren't empty. If a required field
            // is empty, set args.Cancel = true to keep the dialog open.
            if (string.IsNullOrEmpty(userNameTextBox.Text))
            {
                args.Cancel = true;
                errorTextBlock.Text = "User name is required.";
            }
            else if (string.IsNullOrEmpty(passwordTextBox.Password))
            {
                args.Cancel = true;
                errorTextBlock.Text = "Password is required.";
            }

            // If you're performing async operations in the button click handler,
            // get a deferral before you await the operation. Then, complete the
            // deferral when the async operation is complete.

            ContentDialogButtonClickDeferral deferral = args.GetDeferral();    
            if (await SomeAsyncSignInOperation())
            {
                this.Result = SignInResult.SignInOK;
            }
            else
            {
                this.Result = SignInResult.SignInFail;
            }
            deferral.Complete();
        }

        private void ContentDialog_SecondaryButtonClick(ContentDialog sender, ContentDialogButtonClickEventArgs args)
        {
            // User clicked Cancel.
            this.Result = SignInResult.SignInCancel;
        }

        void SignInContentDialog_Opened(ContentDialog sender, ContentDialogOpenedEventArgs args)
        {
            this.Result = SignInResult.Nothing;

            // If the user name is saved, get it and populate the user name field.
            Windows.Storage.ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
            if (roamingSettings.Values.ContainsKey("userName"))
            {
                userNameTextBox.Text = roamingSettings.Values["userName"].ToString();
                saveUserNameCheckBox.IsChecked = true;
            }
        }

        void SignInContentDialog_Closing(ContentDialog sender, ContentDialogClosingEventArgs args)
        {
            // If sign in was successful, save or clear the user name based on the user choice.
            if (this.Result == SignInResult.SignInOK)
            {
                if (saveUserNameCheckBox.IsChecked == true)
                {
                    SaveUserName();
                }
                else
                {
                    ClearUserName();
                }
            }

            // If the user entered a name and checked or cleared the 'save user name' checkbox, then clicked the back arrow,
            // confirm if it was their intention to save or clear the user name without signing in. 
            if (this.Result == SignInResult.Nothing && !string.IsNullOrEmpty(userNameTextBox.Text))
            {
                if (saveUserNameCheckBox.IsChecked == false)
                {
                    args.Cancel = true;
                    FlyoutBase.SetAttachedFlyout(this, (FlyoutBase)this.Resources["DiscardNameFlyout"]);
                    FlyoutBase.ShowAttachedFlyout(this);
                }
                else if (saveUserNameCheckBox.IsChecked == true && !string.IsNullOrEmpty(userNameTextBox.Text))
                {
                    args.Cancel = true;
                    FlyoutBase.SetAttachedFlyout(this, (FlyoutBase)this.Resources["SaveNameFlyout"]);
                    FlyoutBase.ShowAttachedFlyout(this);
                }
            }
        }

        private void SaveUserName()
        {
            Windows.Storage.ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
            roamingSettings.Values["userName"] = userNameTextBox.Text;
        }

        private void ClearUserName()
        {
            Windows.Storage.ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
            roamingSettings.Values["userName"] = null;
            userNameTextBox.Text = string.Empty;
        }

        // Handle the button clicks from the flyouts.
        private void SaveButton_Click(object sender, RoutedEventArgs e)
        {
            SaveUserName();
            FlyoutBase.GetAttachedFlyout(this).Hide();
        }

        private void DiscardButton_Click(object sender, RoutedEventArgs e)
        {
            ClearUserName();
            FlyoutBase.GetAttachedFlyout(this).Hide();
        }

        // When the flyout closes, hide the sign in dialog, too.
        private void Flyout_Closed(object sender, object e)
        {
            this.Hide();
        }
    }
}

```

Here's code that shows the `SignInContentDialog` and uses it's custom `SignInResult`.

```csharp

private async void ShowSignInDialogButton_Click(object sender, RoutedEventArgs e)
{
    SignInContentDialog signInDialog = new SignInContentDialog();
    await signInDialog.ShowAsync();

    if (signInDialog.Result == SignInResult.SignInOK)
    {
        // Sign in was successful.
    }
    else if (signInDialog.Result == SignInResult.SignInFail)
    {
        // Sign in failed.
    }
    else if (signInDialog.Result == SignInResult.SignInCancel)
    {
        // Sign in was cancelled by the user.
    }
}
```



## -see-also
[ContentControl](contentcontrol.md), [ContentDialog styles and templates](http://msdn.microsoft.com/library/bbaa462d-07bb-4492-b05b-fa056002b298)