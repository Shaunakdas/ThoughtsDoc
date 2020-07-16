<h2>USeCase#1: Question Source</h2>

* Check SoundManager, duplicate SecondaryAudioSource as VoiceAudioSource.

* SoundManager.cs changes
```
#L13
public AudioSource _voiceAudioSource;
```        

* Assign VoiceAudioSource gameobject to **Voice audio source** in SoundManager script of SoundManager gameobject.

* SoundManager.cs



```

    public void StartVoiceAudio(string _urlPath)
    {
        StartCoroutine(SongCoroutine(_voiceAudioSource, _urlPath));
    }
    public void StopVoiceAudio()
    {
        _voiceAudioSource.Stop();
    }
    IEnumerator SongCoroutine(AudioSource _source, string _urlPath)
    {
        using (UnityWebRequest www = UnityWebRequestMultimedia.GetAudioClip(_urlPath, AudioType.WAV))
        {
            DownloadHandlerAudioClip dHA = new DownloadHandlerAudioClip(string.Empty, AudioType.WAV)
            {
                streamAudio = true
            };
            www.downloadHandler = dHA;
            www.SendWebRequest();
            while (www.downloadProgress < 0.2)
            {
                Debug.Log(www.downloadProgress);
                yield return new WaitForSeconds(.1f);
            }
            if (www.isNetworkError)
            {
                Debug.Log("error");
            }
            else
            {
                _source.clip = dHA.audioClip;
                _source.Play();
            }
        }
    }
```

<h2>USeCase#2: Dialog System</h2>

* DialogSystem.cs

```
#L43
private string m_current_audio_url;

#L548
m_current_audio_url = m_dialogues.character_dialogs[m_commentno].audio_url;

IEnumerator _Tallk()
    {
....
    SoundManager.Instance.StartVoiceAudio(m_current_audio_url);

}

void _SkipComment(){
    SoundManager.Instance.StopVoiceAudio();
}

void _GoHome()
    {
    SoundManager.Instance.StopVoiceAudio();
}

void _NextComment()
    {
    SoundManager.Instance.StopVoiceAudio();
    }
    
public void _Close()
    {
    SoundManager.Instance.StopVoiceAudio();
    }
```
* Hintcontroller.cs

```
#L46
private string m_current_audio_url;

#L182
m_current_audio_url = m_hintstructure.pages[m_curruntpage].audios[m_curruntline]

void _BlueDotMethod()
    {
...
    m_current_audio_url = m_hintstructure.pages[m_curruntpage].audios[m_curruntline]
    }
IEnumerator _TypeWriterEffect()
    {
    ...
    SoundManager.Instance.StartVoiceAudio(m_current_audio_url);
    
    }

void _NextButton()
    {
    SoundManager.Instance.StopVoiceAudio();
    }
void _PriviousButton()
    {
    SoundManager.Instance.StopVoiceAudio();
    }
void _FinishMethod()
    {
    SoundManager.Instance.StopVoiceAudio();
    }
void _SkipMethod()
    {
    SoundManager.Instance.StopVoiceAudio();
    }
```

* ConversionGameController.cs

```
IEnumerator NewSectionAnimation(float waittime)
        {
        
```
