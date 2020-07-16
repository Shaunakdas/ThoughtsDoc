<h2>USeCase#1: Question Source</h2>
* Check SoundManager, duplicate Audio Source and assign new audioSource gameobject to audio source in soundmanager field.

* SoundManager.cs



```

    public void StartQuestionAudio(string _urlPath)
    {
        StartCoroutine(SongCoroutine(questionAudioSource, _urlPath));
    }
    public void StopQuestionAudio()
    {
        questionAudioSource.Stop();
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

ConversionGamecontroller.cs
