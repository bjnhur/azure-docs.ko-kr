---
title: '빠른 시작: 음성 SDK를 사용 하 여 사용자 지정 명령 앱에 연결-음성 서비스'
titleSuffix: Azure Cognitive Services
description: 이 문서에서는 사용자 지정 명령을 사용 하 여 음성 SDK 클라이언트 응용 프로그램을 만듭니다.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 9e324af0b90f595b5b7af2a417a562efb193d854
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156780"
---
# <a name="quickstart-connect-to-a-custom-commands-application-with-the-speech-sdk-preview"></a>빠른 시작: 음성 SDK (미리 보기)를 사용 하 여 사용자 지정 명령 응용 프로그램에 연결

호스트 된 사용자 지정 명령 응용 프로그램을 만든 후 클라이언트 장치에서 해당 응용 프로그램에 대 한 통신을 시작할 수 있습니다.

이 문서에서는 다음을 수행 합니다.

- 사용자 지정 명령 응용 프로그램 게시 및 응용 프로그램 식별자 가져오기 (앱 ID)
- 음성 SDK를 사용 하 여 클라이언트 앱을 만들어 사용자 지정 명령 응용 프로그램과 통신할 수 있도록 합니다.

## <a name="prerequisites"></a>필수 조건

이 문서를 완료 하려면 사용자 지정 명령 응용 프로그램이 필요 합니다. 사용자 지정 명령 응용 프로그램을 아직 만들지 않은 경우 이러한 이전 빠른 시작에서 수행할 수 있습니다.

- [빠른 시작: 사용자 지정 명령 만들기 (미리 보기)](./quickstart-custom-speech-commands-create-new.md)
- [빠른 시작: 매개 변수를 사용 하 여 사용자 지정 명령 만들기 (미리 보기)](./quickstart-custom-speech-commands-create-parameters.md)

다음도 필요 합니다.

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
- Speech Service에 대한 Azure 구독 키. [무료로 다운로드](get-started.md) 하거나 [Azure Portal](https://portal.azure.com) 에서 만드세요.

## <a name="optional-get-started-fast"></a>선택 사항: 빠르게 시작

이 빠른 시작에서는 클라이언트 응용 프로그램을 사용자 지정 명령 앱에 연결 하도록 만드는 방법을 단계별로 설명 합니다. 바로 시작하려면 이 빠른 시작에 사용되는 즉시 컴파일 가능한 완전한 소스 코드를 사용하면 됩니다. 이 소스 코드는 `quickstart` 폴더의 [Speech SDK 샘플](https://aka.ms/csspeech/samples) 아래에 있습니다.

## <a name="step-1-publish-custom-commands-application"></a>1 단계: 사용자 지정 명령 응용 프로그램 게시

1. [이전에 만든 사용자 지정 명령 응용 프로그램](./quickstart-custom-speech-commands-create-new.md) 을 열고 **게시** 를 선택 합니다.

   > [!div class="mx-imgBorder"]
   > 응용 프로그램](media/custom-speech-commands/fulfill-sdk-publish-application.png) ![게시

1. 나중에 사용 하기 위해 게시 알림에서 앱 ID를 복사 합니다.

## <a name="step-2-create-a-visual-studio-project"></a>2 단계: Visual Studio 프로젝트 만들기

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="step-3-add-sample-code"></a>3 단계: 샘플 코드 추가

이 단계에서는 응용 프로그램의 사용자 인터페이스를 정의 하는 XAML 코드를 추가 하 고 C# 코드를 사용 하는 구현을 추가 합니다.

### <a name="xaml-code"></a>XAML 코드

XAML 코드를 추가 하 여 응용 프로그램의 사용자 인터페이스를 만듭니다.

1. **솔루션 탐색기**에서 `MainPage.xaml`를 엽니다.

1. 디자이너의 XAML 보기에서 전체 콘텐츠를 다음 코드 조각으로 바꿉니다.

   ```xml
   <Page
       x:Class="helloworld.MainPage"
       xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       xmlns:local="using:helloworld"
       xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
       xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
       mc:Ignorable="d"
       Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

       <Grid>
           <StackPanel Orientation="Vertical" HorizontalAlignment="Center"
                       Margin="20,50,0,0" VerticalAlignment="Center" Width="800">
               <Button x:Name="EnableMicrophoneButton" Content="Enable Microphone"
                       Margin="0,0,10,0" Click="EnableMicrophone_ButtonClicked"
                       Height="35"/>
               <Button x:Name="ListenButton" Content="Talk"
                       Margin="0,10,10,0" Click="ListenButton_ButtonClicked"
                       Height="35"/>
               <StackPanel x:Name="StatusPanel" Orientation="Vertical"
                           RelativePanel.AlignBottomWithPanel="True"
                           RelativePanel.AlignRightWithPanel="True"
                           RelativePanel.AlignLeftWithPanel="True">
                   <TextBlock x:Name="StatusLabel" Margin="0,10,10,0"
                              TextWrapping="Wrap" Text="Status:" FontSize="20"/>
                   <Border x:Name="StatusBorder" Margin="0,0,0,0">
                       <ScrollViewer VerticalScrollMode="Auto"
                                     VerticalScrollBarVisibility="Auto" MaxHeight="200">
                           <!-- Use LiveSetting to enable screen readers to announce
                                the status update. -->
                           <TextBlock
                               x:Name="StatusBlock" FontWeight="Bold"
                               AutomationProperties.LiveSetting="Assertive"
                               MaxWidth="{Binding ElementName=Splitter, Path=ActualWidth}"
                               Margin="10,10,10,20" TextWrapping="Wrap"  />
                       </ScrollViewer>
                   </Border>
               </StackPanel>
           </StackPanel>
           <MediaElement x:Name="mediaElement"/>
       </Grid>
   </Page>
   ```

애플리케이션의 사용자 인터페이스를 표시하도록 디자인 보기가 업데이트됩니다.

### <a name="c-code-behind-source"></a>C# 코드 숨김 원본

응용 프로그램이 예상 대로 작동 하도록 코드 숨김이 소스를 추가 합니다. 코드 숨김 원본은 다음을 포함합니다.

- `Speech` 및 `Speech.Dialog` 네임 스페이스에 대 한 필수 `using` 문
- 단추 처리기에 연결된 마이크 액세스를 보장하는 간단한 구현
- 애플리케이션에서 메시지 및 오류를 표시하는 기본 UI 도우미
- 나중에 채울 초기화 코드 경로에 대한 시작 지점
- 텍스트 음성 변환을 재생할 수 있는 도우미(스트리밍 지원 없음)
- 나중에 채울 수신 대기를 시작하는 빈 단추 처리기

다음과 같이 코드의 소스를 추가 합니다.

1. **솔루션 탐색기**에서 `MainPage.xaml.cs` `MainPage.xaml`에 그룹화 된 코드 숨겨진 소스 파일을 엽니다.

1. 파일의 내용을 다음 코드로 바꿉니다.

   ```csharp
   using Microsoft.CognitiveServices.Speech;
   using Microsoft.CognitiveServices.Speech.Audio;
   using Microsoft.CognitiveServices.Speech.Dialog;
   using System;
   using System.Diagnostics;
   using System.IO;
   using System.Text;
   using Windows.Foundation;
   using Windows.Storage.Streams;
   using Windows.UI.Xaml;
   using Windows.UI.Xaml.Controls;
   using Windows.UI.Xaml.Media;

   namespace helloworld
   {
       public sealed partial class MainPage : Page
       {
           private DialogServiceConnector connector;

           private enum NotifyType
           {
               StatusMessage,
               ErrorMessage
           };

           public MainPage()
           {
               this.InitializeComponent();
           }

           private async void EnableMicrophone_ButtonClicked(
               object sender, RoutedEventArgs e)
           {
               bool isMicAvailable = true;
               try
               {
                   var mediaCapture = new Windows.Media.Capture.MediaCapture();
                   var settings =
                       new Windows.Media.Capture.MediaCaptureInitializationSettings();
                   settings.StreamingCaptureMode =
                       Windows.Media.Capture.StreamingCaptureMode.Audio;
                   await mediaCapture.InitializeAsync(settings);
               }
               catch (Exception)
               {
                   isMicAvailable = false;
               }
               if (!isMicAvailable)
               {
                   await Windows.System.Launcher.LaunchUriAsync(
                       new Uri("ms-settings:privacy-microphone"));
               }
               else
               {
                   NotifyUser("Microphone was enabled", NotifyType.StatusMessage);
               }
           }

           private void NotifyUser(
               string strMessage, NotifyType type = NotifyType.StatusMessage)
           {
               // If called from the UI thread, then update immediately.
               // Otherwise, schedule a task on the UI thread to perform the update.
               if (Dispatcher.HasThreadAccess)
               {
                   UpdateStatus(strMessage, type);
               }
               else
               {
                   var task = Dispatcher.RunAsync(
                       Windows.UI.Core.CoreDispatcherPriority.Normal,
                       () => UpdateStatus(strMessage, type));
               }
           }

           private void UpdateStatus(string strMessage, NotifyType type)
           {
               switch (type)
               {
                   case NotifyType.StatusMessage:
                       StatusBorder.Background = new SolidColorBrush(
                           Windows.UI.Colors.Green);
                       break;
                   case NotifyType.ErrorMessage:
                       StatusBorder.Background = new SolidColorBrush(
                           Windows.UI.Colors.Red);
                       break;
               }
               StatusBlock.Text += string.IsNullOrEmpty(StatusBlock.Text)
                   ? strMessage : "\n" + strMessage;

               if (!string.IsNullOrEmpty(StatusBlock.Text))
               {
                   StatusBorder.Visibility = Visibility.Visible;
                   StatusPanel.Visibility = Visibility.Visible;
               }
               else
               {
                   StatusBorder.Visibility = Visibility.Collapsed;
                   StatusPanel.Visibility = Visibility.Collapsed;
               }
               // Raise an event if necessary to enable a screen reader
               // to announce the status update.
               var peer = Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer.FromElement(StatusBlock);
               if (peer != null)
               {
                   peer.RaiseAutomationEvent(
                       Windows.UI.Xaml.Automation.Peers.AutomationEvents.LiveRegionChanged);
               }
           }

           // Waits for and accumulates all audio associated with a given
           // PullAudioOutputStream and then plays it to the MediaElement. Long spoken
           // audio will create extra latency and a streaming playback solution
           // (that plays audio while it continues to be received) should be used --
           // see the samples for examples of this.
           private void SynchronouslyPlayActivityAudio(
               PullAudioOutputStream activityAudio)
           {
               var playbackStreamWithHeader = new MemoryStream();
               playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("RIFF"), 0, 4); // ChunkID
               playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // ChunkSize: max
               playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("WAVE"), 0, 4); // Format
               playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("fmt "), 0, 4); // Subchunk1ID
               playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 4); // Subchunk1Size: PCM
               playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // AudioFormat: PCM
               playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // NumChannels: mono
               playbackStreamWithHeader.Write(BitConverter.GetBytes(16000), 0, 4); // SampleRate: 16kHz
               playbackStreamWithHeader.Write(BitConverter.GetBytes(32000), 0, 4); // ByteRate
               playbackStreamWithHeader.Write(BitConverter.GetBytes(2), 0, 2); // BlockAlign
               playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 2); // BitsPerSample: 16-bit
               playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("data"), 0, 4); // Subchunk2ID
               playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // Subchunk2Size

               byte[] pullBuffer = new byte[2056];

               uint lastRead = 0;
               do
               {
                   lastRead = activityAudio.Read(pullBuffer);
                   playbackStreamWithHeader.Write(pullBuffer, 0, (int)lastRead);
               }
               while (lastRead == pullBuffer.Length);

               var task = Dispatcher.RunAsync(
                   Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                   mediaElement.SetSource(
                       playbackStreamWithHeader.AsRandomAccessStream(), "audio/wav");
                   mediaElement.Play();
               });
           }

           private void InitializeDialogServiceConnector()
           {
               // New code will go here
           }

           private async void ListenButton_ButtonClicked(
               object sender, RoutedEventArgs e)
           {
               // New code will go here
           }
       }
   }
   ```

1. 의 메서드 본문에 다음 코드를 추가 `InitializeDialogServiceConnector`

   ```csharp
   // This code creates the `DialogServiceConnector` with your subscription information.
   // create a DialogServiceConfig by providing a Custom Commands application id and Cognitive Services subscription key
   // the RecoLanguage property is optional (default en-US); note that only en-US is supported in Preview
   const string speechCommandsApplicationId = "YourApplicationId"; // Your application id
   const string speechSubscriptionKey = "YourSpeechSubscriptionKey"; // Your subscription key
   const string region = "YourServiceRegion"; // The subscription service region. Note: only 'westus2' is currently supported

   var speechCommandsConfig = CustomCommandsConfig.FromSubscription(speechCommandsApplicationId, speechSubscriptionKey, region);
   speechCommandsConfig.SetProperty(PropertyId.SpeechServiceConnection_RecoLanguage, "en-us");
   connector = new DialogServiceConnector(speechCommandsConfig);
   ```

1. `YourApplicationId`, `YourSpeechSubscriptionKey`및 `YourServiceRegion` 문자열을 앱, 음성 구독 및 [지역](regions.md) 에 대 한 사용자 고유의 값으로 바꿉니다.

1. 다음 코드 조각을의 메서드 본문 끝에 추가 `InitializeDialogServiceConnector`

   ```csharp
   //
   // This code sets up handlers for events relied on by `DialogServiceConnector` to communicate its activities,
   // speech recognition results, and other information.
   //
   // ActivityReceived is the main way your client will receive messages, audio, and events
   connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
   {
       NotifyUser(
           $"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

       if (activityReceivedEventArgs.HasAudio)
       {
           SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
       }
   };

   // Canceled will be signaled when a turn is aborted or experiences an error condition
   connector.Canceled += (sender, canceledEventArgs) =>
   {
       NotifyUser($"Canceled, reason={canceledEventArgs.Reason}");
       if (canceledEventArgs.Reason == CancellationReason.Error)
       {
           NotifyUser(
               $"Error: code={canceledEventArgs.ErrorCode}, details={canceledEventArgs.ErrorDetails}");
       }
   };

   // Recognizing (not 'Recognized') will provide the intermediate recognized text
   // while an audio stream is being processed
   connector.Recognizing += (sender, recognitionEventArgs) =>
   {
       NotifyUser($"Recognizing! in-progress text={recognitionEventArgs.Result.Text}");
   };

   // Recognized (not 'Recognizing') will provide the final recognized text
   // once audio capture is completed
   connector.Recognized += (sender, recognitionEventArgs) =>
   {
       NotifyUser($"Final speech-to-text result: '{recognitionEventArgs.Result.Text}'");
   };

   // SessionStarted will notify when audio begins flowing to the service for a turn
   connector.SessionStarted += (sender, sessionEventArgs) =>
   {
       NotifyUser($"Now Listening! Session started, id={sessionEventArgs.SessionId}");
   };

   // SessionStopped will notify when a turn is complete and
   // it's safe to begin listening again
   connector.SessionStopped += (sender, sessionEventArgs) =>
   {
       NotifyUser($"Listening complete. Session ended, id={sessionEventArgs.SessionId}");
   };
   ```

1. `MainPage` 클래스에서 `ListenButton_ButtonClicked` 메서드의 본문에 다음 코드 조각을 추가 합니다.

   ```csharp
   // This code sets up `DialogServiceConnector` to listen, since you already established the configuration and
   // registered the event handlers.
   if (connector == null)
   {
       InitializeDialogServiceConnector();
       // Optional step to speed up first interaction: if not called,
       // connection happens automatically on first use
       var connectTask = connector.ConnectAsync();
   }

   try
   {
       // Start sending audio
       await connector.ListenOnceAsync();
   }
   catch (Exception ex)
   {
       NotifyUser($"Exception: {ex.ToString()}", NotifyType.ErrorMessage);
   }
   ```

1. 메뉴 모음에서 **파일** > **모두 저장** 을 선택 하 여 변경 내용을 저장 합니다.

## <a name="build-and-run-the-application"></a>애플리케이션 빌드 및 실행

1. 메뉴 모음에서 **빌드** > **솔루션 빌드**를 선택하여 애플리케이션을 빌드합니다. 코드는 오류 없이 컴파일됩니다.

1. **디버그** > **디버깅 시작**을 선택하거나, **F5** 키를 눌러 애플리케이션을 시작합니다. **helloworld** 창이 나타납니다.

   ![C#의 샘플 UWP 가상 길잡이 애플리케이션 - 빠른 시작](media/sdk/qs-voice-assistant-uwp-helloworld-window.png)

1. **마이크 사용**을 선택합니다. 액세스 권한 요청이 팝업 되 면 **예**를 선택 합니다.

   ![마이크 액세스 권한 요청](media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. 대화를 선택 하 고 장치 마이크에 **대해**영어 문구 또는 문장을 말합니다. 음성은 Direct Line Speech 채널로 전송되어 텍스트로 전사되고 창에 표시됩니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [방법: 음성 SDK를 사용 하 여 클라이언트에서 명령 수행 (미리 보기)](./how-to-custom-speech-commands-fulfill-sdk.md)
> [방법: 사용자 지정 명령 매개 변수에 유효성 검사 추가 (미리 보기)](./how-to-custom-speech-commands-validations.md)
