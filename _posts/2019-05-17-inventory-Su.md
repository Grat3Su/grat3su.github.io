---
title: "make Inventory"
date: 2019-05-17
comment: false

---



### 벡터를 이용한 인벤토리 제작

```c++
#include<iostream>
#include<conio.h>
#include<vector>

using namespace std;
/*
플레이어는 상점 혹은 던전에서 아이템 구입, 아이템 습득 인벤 저장
플레이어 돈으로 아이템 구매
인벤 자동 저장
인벤 풀
인벤 아이템 지정 사용, 판매, 버리기
*/

enum main {INVENTORY, SHOP, DUNGEON };
enum shop { BUY, SELL };


void PlayerMenu();

class Item {
public:
	int item_code;
	char item_name[20];
	int price;
};


class Inventory {//벡터로 구현
public:
	vector <Item> slot;
};

class Player :public Inventory
{
public:
	int money;
};

void Drop();
Player p;
Player s;
Item menu;
vector<Item>::iterator iterpos;
void PlayerMenu();

void ShopItem()
{
	menu = { 0, "엑스칼리버(모형)", 10 };
	s.slot.push_back(menu);
	menu = { 1,"공인중개사_합격은",50 };
	s.slot.push_back(menu);
	menu = { 2,"곰인형(포악)",100 };
	s.slot.push_back(menu);
	menu = { 3,"마법의_고양이_인형",1050 };
	s.slot.push_back(menu);
	menu = { 4,"멍멍현자의_젤리",1500 };
	s.slot.push_back(menu);
	menu = { 5,"갈아만든_딸기주스",1000 };
	s.slot.push_back(menu);

	
}

void Sell()
{
	if (p.slot.empty())
	{
		cout << "이미 비어있습니다" << endl;
		return;
	}
	cout << "판매할 아이템을 선택하세요" << endl;

	for (int i = 0; i < p.slot.size(); i++)
		cout << i<<" " << p.slot[i].item_name << " ";
	puts("");

	int choice;
	cin >> choice;
	//Item insert_item = s.slot.at(choice);


	if (choice < p.slot.size())
	{
		iterpos = p.slot.begin();

		for (int i = 0; i < choice; i++)
			iterpos++;

		p.money += p.slot[choice].price;
		p.slot.erase(iterpos);


		cout << "판매되었습니다." << endl << "남은 돈 : " << p.money << endl;

		puts("");
	}
	else
		cout << "잘못된 입력값 입니다." << endl;

}

void Buy()
{
	for (int i = 0; i < 6; i++)
	{
		cout << s.slot[i].item_code << " " << s.slot[i].item_name << "\t" << s.slot[i].price << endl;

	}

	int choice;
	cin >> choice;
	Item insert_item = s.slot.at(choice);

	if (p.slot.size() < 6)
	{
		if (p.money - s.slot.at(choice).price >= 0)
		{
			p.slot.push_back(insert_item);		
			p.money -= s.slot.at(choice).price;

			PlayerMenu();
		}
		else
			cout << "돈이 부족합니다" << endl;
	}
	else
	{
		cout << "가방이 다 찼습니다!" << endl;
		cout << "버리기 : 0 | 판매 : 1 | 나가기 : 3" << endl;
		int choice;
		cin >> choice;

		switch (choice)
		{
		case 0:
			Drop();
			break;

		case SELL:
			Sell();
			break;

		default:
			break;
		}
	}

	puts("");

}

void Shop()
{
	cout << "구매 : 0 | 판매 : 1"<<endl;
	int choice;
	cin >> choice;

	switch (choice)
	{
	case BUY:

		Buy();
		break;

	case SELL:
		Sell();
		break;

	default:
		break;
	}	
}

void Move()
{
	int dmove;

	dmove = rand() % 9;

	if (dmove < 6)
	{
		cout << "아이템 발견!" << endl;
		
		if (p.slot.size() < 6)
		{
			Item it = s.slot.at(dmove);
			p.slot.push_back(it);
		}
		else
		{
			cout << "인벤토리가 모두 찼습니다." << endl;
			cout << "버리기 : 0 | 나가기 : 1" << endl;
			
			int choice;
			cin >> choice;


			switch (choice)
			{
			case 0:
				Drop();
				Item it = s.slot.at(dmove);
				p.slot.push_back(it);
				break;
			default:
				break;
			}

		}
		
	}
	else
	{
		cout << "몬스터와 조우!\n" << "당신은 도망쳤다!" << endl;
		cout << "아이템을 잃었습니다." << endl;
		p.slot.pop_back();
	}

	PlayerMenu();
}

void DunGeon()
{
	cout << "0 : 움직임 | 1. 그만두기" << endl;

	//몬스터 조우,	보물 상자 만남

	int choice;
	cin >> choice;
	

	switch (choice)
	{
	case 0:
		Move();
		break;
	default:
		break;
	}

	
}

void Drop()
{
	cout << "버릴 아이템을 선택하세요" << endl;

	for (int i = 0; i < p.slot.size(); i++)
		cout << i << " " << p.slot[i].item_name << " ";
	puts("");

	int choice;
	cin >> choice;
	//Item insert_item = s.slot.at(choice);

	iterpos = p.slot.begin();

	for (int i = 0; i < choice; i++)
		iterpos++;

	
	p.slot.erase(iterpos);


	cout << "버렸습니다." << endl;
	for (int i = 0; i < p.slot.size(); i++)
		cout << i << " " << p.slot[i].item_name << " ";
	puts("");

	puts("");

}

void PlayerMenu()
{
	cout << "현재 인벤토리" << endl;
	
	if (p.slot.empty() == false)
	{
		for (int i = 0; i < p.slot.size(); i++)
			cout << " " << p.slot[i].item_name << " ";
	}
	else
		cout << "비었습니다";
		cout << endl;

		
	
	cout << "소지금 : " <<p.money << endl;
}

int main()
{
	p.money = 1000;	
	iterpos = p.slot.begin();

	ShopItem();

	while (1)
	{
		int choice = 0;
		cout << "0 : 인벤토리 / 1 : 상점 / 2 : 던전" << endl;
		cin >> choice;

		switch (choice)
		{
			
		case INVENTORY:
			
			PlayerMenu();
			break;
		case SHOP:
			Shop();
			break;
		case DUNGEON:
			DunGeon();
			break;
		default:
			cout << "시스템 종료" << endl;
			return 0;
			break;
		}
		cout << endl;
	}
	_getch();
	return 0;
}
```



### deque를 이용한 undo / redo 만들기



```c++
#include<iostream>
#include<conio.h>
#include<deque>
#include<time.h>

using namespace std;

void main()
{
	int rrand;
	int _count = 0;//실행 카운트
	deque <int> dec1;
	deque <int> dec2;
srand(time(0));
	for (int i = 0; i < 10; i++)
	{
		
		rrand = rand() % 985;
		dec1.push_back(rrand);
	}
	while (1)
	{
		for (int i = 0; i < dec1.size(); i++)
			cout << dec1[i] << " ";
		cout << endl;

		for (int i = 0; i < dec2.size(); i++)
			cout << dec2[i] << " ";
		cout << endl;

		int choice;
				
		cout << "start : 0  redo : 1  undo : 2  exit : 3" << endl;
		cin >> choice;

		switch (choice)
		{		
		case 0:
			if (dec1.size() != 0)
			{
				cout << "start" << endl;
				dec2.push_back(dec1.front());
				dec1.pop_front();
			}
			else
			{
			cout << "실행할 명령이 없습니다." << endl;

			int exe;
			while (1)
			{
				cout << "명령을 추가해주세요 : ";
				cin >> exe;
				if (exe > 0)
				{
					for (int i = 0; i < exe; i++)
					{
						rrand = rand() % 985;
						dec1.push_back(rrand);
					}
					break;
				}
				else
				{
					cout << "다시 입력해주세요" << endl;
				}
			}
			}
			_count++;
			break;
		case 1://redo
			if (_count)
			{
				if (dec1.size() != 0)
				{
					cout << choice << " : ";

					cout << "redo" << endl;

					dec2.push_back(dec1.front());
					dec1.pop_front();
					_count--;
				}
				else
				{
					cout << "실행할 명령이 없습니다." << endl;
				}
			}
			else
				cout << "실행할 명령이 없습니다." << endl;
				break;

		case 2:	//undo
			if (dec2.size() != 0)
			{
				cout << choice << " : ";

				cout << "undo" << endl;;
				dec1.push_front(dec2.back());
				dec2.pop_back();
			}
			else
			{
				cout << "실행할 명령이 없습니다." << endl;
				_count = 0;
			}
			
			break;

		case 3:
			cout << "프로그램 종료!";
			_getch();
			return;
					
		}
		cout << endl;

		cout << "dec 1 : ";
		for (int i = 0; i < dec1.size(); i++)
			cout << dec1[i] << " ";
		cout << endl;

		cout << "dec 2 : ";
		for (int i = 0; i < dec2.size(); i++)
			cout << dec2[i] << " " ;
		cout << endl;
		cout << "===================================================================" << endl;
		cout << endl;
		
	}
	
}
```

