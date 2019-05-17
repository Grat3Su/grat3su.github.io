---

title: "NetWork-middle-sub"
date: 2019-05-17
comment: false

---


### ShootBullet.h

```c++
#pragma once
#include "FighterPlane.h"

class ShootBullet : public CFighterPlane
{
protected:
	dsTexture	*m_pFighter;
	RECT		 m_rcImage;
	float        m_xPos;
	float        m_yPos;
	bool		shoot_true;
	float        m_speed;

public:
	ShootBullet();
	ShootBullet(string name);
	~ShootBullet();
	
	BOOL Check_shoot();
	void Shoot_T(bool shoot);

	float GetPosition_X();
	float GetPosition_Y();
};
```



### ShootBullet.cpp

```c++
float ShootBullet::GetPosition_X()
{
	return m_xPos;
}

float ShootBullet::GetPosition_Y()
{
	return m_yPos;
}	

BOOL ShootBullet::Check_shoot()
{
	return shoot_true;
}

void ShootBullet::Shoot_T(bool shoot)
{
	shoot_true = shoot;	
}
```



-나머지는 교수님께서 주신  CFighterPlane와 같음



### packet.h

```c++
#define MOVE_S		0x00000001
#define MOVE_C		0x00000002

#define SHOOT_S	0x00000011
#define SHOOT_C	0x00000012

typedef struct _tgPacketHeader
{
	DWORD	PktID;
	WORD	PktSize;
	float x;
	float y;
}PACKETHEADER;
```







### OpenGL_Templete.cpp

```c++
CFighterPlane I_Win("I_Win");
CFighterPlane I_Lose("I_Lose");

CFighterPlane g_myAir("myAir");
ShootBullet mybullet("mybullet");
ShootBullet recbullet("recbullet");
CFighterPlane g_recAir("recAir");
```



```c++

	g_myAir.Create("./data/1942.png", 5, 323, 67, 49);
	g_recAir.Create("./data/1942.png", 5, 323, 67, 49);
	mybullet.Create("./data/1942.png", 89, 82, 8, 13);
	recbullet.Create("./data/1942.png", 89, 82, 8, 13);
	I_Win.Create("./data/win.png", 0, 0, 230, 215);
	I_Lose.Create("./data/lose.png", 0, 0, 271, 218);
	recbullet.SetPosition(-1, 800);
	mybullet.SetPosition(-1, 800);
	g_recAir.SetPosition(200, 0);
	I_Win.SetPosition(200, 150);
	I_Lose.SetPosition(180, 150);
```



```C++
switch (WSAGETSELECTEVENT(lParam))
		{
		case FD_READ:
		{
			#define BUFSIZE  1024

			SOCKADDR_IN clientaddr;
			char buf[1024];
			int addrlen = sizeof(clientaddr);
			int retval = recvfrom(g_hSocket, buf, BUFSIZE,
				0, (SOCKADDR *)&clientaddr, &addrlen);
			if (retval == SOCKET_ERROR)
			{
				if (WSAGetLastError() != WSAEWOULDBLOCK)
				{
				}
				break;
			}
			else
			{
				accept_ = true;
			}

			g_Queue.OnPutData(buf, retval);
			PACKETHEADER *pHeader = g_Queue.GetPacket();
			while (pHeader != NULL)
			{
				switch (pHeader->PktID)//받았을 때. move sc, shoot sc 
				{
				case MOVE_S://여기서 받을거 넣기
					g_recAir.SetPosition(pHeader->x, 0);

				
					break;
				case SHOOT_S:
					recbullet.Shoot_T(true);
					recbullet.SetPosition(g_recAir.GetPosition_X() + 10, g_recAir.GetPosition_Y());
					break;
					//case : break;
				}
				g_Queue.OnPopData(pHeader->PktSize);

				pHeader = g_Queue.GetPacket();
			}

		}
		break;
```



```C++

bool Lose = false;
bool Win = false;
int seq = 0;
bool _count = false;
void OnIdle(float deltaTime)
{
	DWORD	tickCount;
	bool move = false;
	bool shoot = false;
	BYTE key[256];
	::GetKeyboardState(key);

	bool GameOver = false;

	float xPos = 0.0f, yPos =0.0f;

	if (Win == true || Lose == true)
		GameOver = true;

	if (GameOver == false)
	{
		if (key[VK_LEFT] & 0x80)
		{
			xPos = -1.0f;
			move = true;
		}
		if (key[VK_RIGHT] & 0x80)
		{
			xPos = 1.0f;
			move = true;
		}
		if (key[VK_ADD] & 0x80)
		{
			g_myAir.ChangeSpeed(10.0f);
			move = true;
		}
		if (key[VK_SUBTRACT] & 0x80)
		{
			g_myAir.ChangeSpeed(-10.0f);
			move = true;
		}
		if (key[VK_SPACE] & 0x80)
		{
			if (mybullet.Check_shoot() == false)
				mybullet.SetPosition(g_myAir.GetPosition_X() + 10, g_myAir.GetPosition_Y());
			mybullet.Shoot_T(true);

			shoot = true;
		}
		g_myAir.Move(xPos, yPos, deltaTime);
	}


	if (mybullet.Check_shoot() == true)//충돌처리 넣기
	{

		if (_count == true)
		{
			shoot = false;
		}_count = true;
		mybullet.Move(0, -1.0f, deltaTime);
		if (mybullet.GetPosition_Y() <= g_recAir.GetPosition_Y())
		{
			if (g_recAir.GetPosition_X() < mybullet.GetPosition_X() && g_recAir.GetPosition_X() + 67 > mybullet.GetPosition_X())
			{
				Win = true;

			}
			mybullet.Shoot_T(false);
			mybullet.SetPosition(0, 800);
			_count = false;
		}
	}


	if (recbullet.Check_shoot() == true)//충돌처리 넣기
	{
		recbullet.Move(0, 1.0f, deltaTime);
		if (recbullet.GetPosition_Y() >= g_myAir.GetPosition_Y())
		{
				if (g_myAir.GetPosition_X() < recbullet.GetPosition_X() && g_myAir.GetPosition_X()+67 > recbullet.GetPosition_X())
				{
					Lose = true;

				}
			
			recbullet.Shoot_T(false);
			recbullet.SetPosition(0, 800);
		}
	}

	g_OpenGL.BeginRender();

	if (Win == true)
	{
		if (seq < 4)
			g_recAir.Create("./data/1942.png", 5 + (seq * 79), 323, 67, 49);
		else
			g_recAir.Create("./data/1942.png", 5 + ((seq - 4) * 79), 378, 67, 49);

		if (seq < 9)
			seq++;	
		else
		{
			I_Win.DrawFighter(0);
		}

		g_myAir.DrawFighter(0);
		g_recAir.DrawFighter(1);
	}

	else if (Lose == true)
	{
		if (seq < 4)
			g_myAir.Create("./data/1942.png", 5 + (seq * 79), 323, 67, 49);
		else
			g_myAir.Create("./data/1942.png", 5 + ((seq - 4) * 79), 378, 67, 49);

		if (seq < 9)
			seq++;
		else
		{
			I_Lose.DrawFighter(0);
		}

		g_myAir.DrawFighter(1);
		g_recAir.DrawFighter(0);

	}

	else
	{
		g_myAir.DrawFighter(0);
		if (accept_ == true)
		g_recAir.DrawFighter(0);
		mybullet.DrawFighter(0);
		recbullet.DrawFighter(0);
	}

	char buf[1024];
	PACKETHEADER* pHeader = (PACKETHEADER*)buf;


	pHeader->x = g_myAir.GetPosition_X();
	pHeader->y = g_myAir.GetPosition_Y();

	pHeader->PktID = MOVE_C;
	pHeader->PktSize = sizeof(PACKETHEADER);
	//Send(g_hSocket, buf, pHeader->PktSize);

	if (shoot == true)
	{
		pHeader->PktID = SHOOT_C;
		pHeader->PktSize = sizeof(PACKETHEADER);

	}

	Send(g_hSocket, buf, pHeader->PktSize);

	g_OpenGL.EndRender(g_hDC);
}
```

