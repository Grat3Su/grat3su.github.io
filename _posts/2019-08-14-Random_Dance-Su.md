---

title : "복사 생성자"
date: 2019-08-14
comment: false
---

### Random Dance

- 8 / 5  ~ 8 / 14



-> 하고 싶었던 것

1. 마우스 포인터 바꾸기

   - 발단 : 다운받은 에셋에 마우스 포인터가 있다!

   ![1565761847622](C:\Users\403\AppData\Roaming\Typora\typora-user-images\1565761847622.png)

   - 전개 : 구글 검색

   ![1565761606183](C:\Users\403\AppData\Roaming\Typora\typora-user-images\1565761606183.png)

   - 위기 : 코드를 알수없음

   ![1565761589403](C:\Users\403\AppData\Roaming\Typora\typora-user-images\1565761589403.png)



![1565761918050](C:\Users\403\AppData\Roaming\Typora\typora-user-images\1565761918050.png)



- 결말

![1565761535550](C:\Users\403\AppData\Roaming\Typora\typora-user-images\1565761535550.png)

#### 성공!



----

2. 게임 기능 만들기
   - CharacterState
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CharacterState : MonoBehaviour
{
    // Start is called before the first frame update
    Animator anim;
    void Start()
    {
        anim = GetComponent<Animator>();
    }

    // Update is called once per frame
    void Update()
    {
        anim.SetInteger("state", 0);
        anim.SetInteger("state", 1);//lose
        anim.SetInteger("state", 2);//win
    }
}

```

   - RandomStart

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RandomStart : MonoBehaviour
{
    int index;
    Vector3[] state;
    public Sprite ArrowShape;
    public bool stop = false;
    int mode;//방향키 체크

    SpriteRenderer sprenderer;

    public float Timer;
    public float EndTimer;
    public float CheckTime = 3.0f;
    public float DelayTime = 0.3f;

    Animator anim;
    public GameObject character;
    // Start is called before the first frame update
    void Start()
    {
        state = new Vector3[4];
        state[0] = new Vector3(0, 0, 0);
        state[1] = new Vector3(0, 0, 180);
        state[2] = new Vector3(0, 0, 90);
        state[3] = new Vector3(0, 0, -90);
        index = Random.Range(0, 4);
        sprenderer = gameObject.GetComponent<SpriteRenderer>();

        anim = character.GetComponent<Animator>();
    }

    // Update is called once per frame
    void Update()
    {
       

        if (stop == false)
        {
            Timer += Time.deltaTime;
            EndTimer += Time.deltaTime;
            if (Timer > DelayTime)
            {
                Timer = 0;
                SwitchSprite();
            }
            if(EndTimer > CheckTime)
            {
                EndTimer = 0;
                stop = true;
            }            
        }
        else//체크
        {
            Timer = 0;
            Check();
        }
        
    }    

    void Check()
    {
        mode = 0;//0 : 아무것도 입력하지 않음. 1 : L | 2: R | 3 : U | 4 : D

        if (Input.GetKey(KeyCode.LeftArrow) == true)
        {
            mode = 1;
        }

        else if (Input.GetKey(KeyCode.RightArrow) == true)
        {
            mode = 3;
        }

        else if (Input.GetKey(KeyCode.DownArrow) == true)
        {
            mode = 4;
        }

        else if (Input.GetKey(KeyCode.UpArrow) == true)
        {
            mode = 2;
        }

        

        if(mode !=0)
        {
            Debug.Log(mode);
            stop = false;
            if (mode.ToString() == gameObject.tag)
            {
                if(DelayTime > 0.1f)
                DelayTime -= 0.025f;
                Debug.Log("맞음!");
                anim.SetInteger("state", 2);
            }
            else
            {
                if (DelayTime < 0.5f)
                    DelayTime += 0.05f;
                Debug.Log("틀림!");
                anim.SetInteger("state", 1);
            }
        }
    }

    void SwitchSprite()
    {
        anim.SetInteger("state", 0);
        switch (index)
        {
            case 0://1
                index = 1;
                gameObject.tag = "2";
                sprenderer.color = Color.red;
                gameObject.transform.rotation = Quaternion.Euler(0,0,90);
                break;

            case 1://2
                index = 2;
                gameObject.tag = "3";
                sprenderer.color = Color.white;
                gameObject.transform.rotation = Quaternion.Euler(0, 0, 0);
                break;

            case 2://3
                index = 3;
                gameObject.tag = "4";
                sprenderer.color = Color.green;
                gameObject.transform.rotation = Quaternion.Euler(0, 0, -90);
                break;

            case 3://4
                index = 0;
                gameObject.tag = "1";
                sprenderer.color = Color.yellow;
                gameObject.transform.rotation = Quaternion.Euler(0, 0, 180);
                break;
        }
       
    }
}
```



   - RotateBox

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RotateBox : MonoBehaviour
{
    public float speed;

    public float rotx = 15;
    public float roty = 30;
    public float rotz = 45;

    // Update is called once per frame
    void Update()
    {
        transform.Rotate(new Vector3(rotx, roty, rotz) * Time.deltaTime*speed);
    }
}
```



---



2. 애니메이션 넣기

![1565762346846](C:\Users\403\AppData\Roaming\Typora\typora-user-images\1565762346846.png)

---

### 넣어야 할 기능

1. 메인메뉴 - 시작 / 나가기
2. 엔딩
3. 멈추기 버튼

---



