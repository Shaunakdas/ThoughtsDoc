```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class SpriteChanger : MonoBehaviour
{
    public Image image;
     public Sprite[] sprites;
        public float animationSpeed;
        
        public IEnumerator nukeMethod()
        {
            for (int j=0; j < 10; j++){
                for (int i=0; i < sprites.Length; i++)
                    {
                        image.sprite = sprites[i];
                        yield return new WaitForSeconds(animationSpeed);
                    }
            }
            //destroy all game objects
            
        }
    // Start is called before the first frame update
    void Start()
    {
        image = gameObject.GetComponent<Image>();
        StartCoroutine(nukeMethod());
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}

```
