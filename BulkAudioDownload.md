```
    public class AudioFile
    {
        public string _urlPath;
        public int _urlCount;
        public UnityWebRequest _urlWww;
        public DownloadHandlerAudioClip _urlDHA;
        public AudioFile(string _urlPath, int count){
            _urlCount = count;
            _urlWww = UnityWebRequestMultimedia.GetAudioClip(_urlPath, AudioType.MPEG);
            _urlDHA = new DownloadHandlerAudioClip(string.Empty, AudioType.MPEG)
            {
                streamAudio = true
            };
            _urlWww.downloadHandler = _urlDHA;
        }
        public IEnumerator DownloadAudio()
        {
            _urlWww.SendWebRequest();
            while (_urlWww.downloadProgress < 0.2)
            {
                Debug.Log(_urlCount + ","+ _urlWww.downloadProgress);
                yield return new WaitForSeconds(.1f);
            }
            if (_urlWww.isNetworkError)
            {
                Debug.Log(_urlCount +"error");
            }
            else
            {
                Debug.Log(_urlCount +"success");
            }
        }
    }
    List<AudioFile> m_urlPaths;
    public void StartBulkVoiceDownload(List<string> _urlPaths){
        int i =0;
        foreach (string _url in _urlPaths)
        {
            AudioFile _file = new AudioFile(_url,i);
            i++;
            m_urlPaths.Add(_file);
            StartCoroutine(_file.DownloadAudio());
        }
    }
    public void PlayAudio(int _urlId){
        AudioFile requested = m_urlPaths[_urlId];
        StartCoroutine(PlaySongCoroutine(_voiceAudioSource, requested));
    }
    IEnumerator PlaySongCoroutine(AudioSource _source, AudioFile _file)
    {
        
        while (_file._urlWww.downloadProgress < 0.2)
        {
            Debug.Log(_file._urlWww.downloadProgress);
            yield return new WaitForSeconds(.1f);
        }
        if (_file._urlWww.isNetworkError)
        {
            Debug.Log("error");
        }
        else
        {
            _source.clip = _file._urlDHA.audioClip;
            _source.Play();
        }
    }
```
