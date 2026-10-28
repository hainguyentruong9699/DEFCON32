---
# System prepended metadata

title: DEFCON 32 (GameHackingVillage CTF 2024)
tags: ['#GameHacking']

---

# DEFCON 32 (GameHackingVillage CTF 2024)
---

![image](https://hackmd.io/_uploads/rkScomxGGe.png)

-- Tổng quan: Đây là một challenge khá thú vị về Game Hacking, được tạo vào năm 2024. Tuy đã cũ, nhưng challenge này vẫn là nền tảng tốt để mình luyện tập thêm về reverse engineering cũng như các kỹ năng liên quan đến game hacking
-- Đồng thời, challenge này mình sẽ sử dụng chủ yếu 2 ứng dụng phổ biến để điều chỉnh thông số cũng như code đó là `Dnspy` và `Cheat Engine`
-- Game gồm có 10 levels (từ cơ bản đến nâng cao), 9 levels bình thường và 1 level secret, bắt đầu vào từng challenge nhé


# Level 1



## Nhiệm vụ

-- Phá hủy **khối băng** bằng cách chạm vào nó 
-- Sau khi thử chơi, chúng ta thấy được là máu của người chơi sẽ ít hơn máu của khối băng

## Hướng giải
-- Sử dụng `Cheat Engine` để chỉnh thông số của game qua các bước sau: ( vì đây là level 1 nên mình sẽ đi chi tiết nhé, về sau sẽ không nói thêm)

-- Đầu tiên, xác định thông số cần chỉnh, ở đây là `100 HP`
![image](https://hackmd.io/_uploads/Hy_oGEgzMg.png)

-- Bước hai, bật `Cheat engine` --> File --> Process List --> GamehackinGG.exe --> Open
![image](https://hackmd.io/_uploads/BJEZmVxGzl.png)

-- Bước ba, nhập giá trị `100`, vì mình biết chính xác giá trị nên chọn `Scan type = exact value` , `Value type= All`, sau đó bấm first scan để scan lần đầu tiên
![image](https://hackmd.io/_uploads/HyHuXNxGMl.png)

-- Bước 4, sau khi đã scan xong, nếu xuất hiện nhiều địa chỉ khác nhau thì chúng ta phải khoanh vùng địa chỉ chứa `HP` bằng cách cho nhân vật mình bị mất máu 1-2 lần rồi bấm `Next scan` cho đến khi chỉ còn chứa 1 địa chỉ offset là giá trị `HP`
![image](https://hackmd.io/_uploads/SyrLEVxMfe.png)

![Screenshot 2026-06-17 220146](https://hackmd.io/_uploads/ByeWPVgGfx.png)

-- Đây chính là offset chứa giá trị thanh HP, sửa thành giá trị xxxxx và tận hưởng thôi

![Screenshot_300KB](https://hackmd.io/_uploads/B1z4_EeMMx.jpg)

--> Flag: GHCTF{this_is_the_first_flag_woot}






# Level 2


## Nhiệm vụ
- Ở level 2 này , chúng ta cần phải tiêu diệt con ngựa bằng khẩu súng ở màn 1, tuy nhiên 10 viên đạn là không đủ :v, thế thì chỉnh như ở level 1 thôi

## Hướng giải
-- Tìm offset chứa giá trị viên đạn, ở đây game cho chúng ta 10 viên đạn
 ![image](https://hackmd.io/_uploads/rJb5eHgGfx.png)
-- Bắn từng viên 1 để tra ra offset và thay đổi giá trị thôi(nhớ là phải scan 10 viên trước rồi mới bắn nhé)
-- Nếu không thu hẹp được 1 offset thì chúng ta có thể thử gán giá trị cho tất cả các offset nhé
 ![image](https://hackmd.io/_uploads/SJumZBxGfl.png)
-- Thành công:
![image](https://hackmd.io/_uploads/BJhB-BlzMl.png)
![image](https://hackmd.io/_uploads/HkxoWSlGGl.png)
-> Flag: GHCTF{oops_sorry_mr_unicorn}
# Level 3

## Nhiệm vụ
-- Tới màn 3, nhiệm vụ của chúng ta là tìm ra cách tiêu diệt con boss 

## Hướng giải
-- Ở level 3, chúng ta chỉ việc kết hợp 2 thứ mà chúng ta đã làm ở 2 level đầu
-- Còn lại các bạn chỉ việc xoay vòng vòng đồng thời nhấn nút shift để bắn con boss thui là xog rùi
![image](https://hackmd.io/_uploads/SJKdL_gzfx.png)
--> Flag: GHCTF{the_best_defense_is_offense}

# Level 4

## Nhiệm vụ
- Ở màn này, chúng ta cần phải tìm được cách để nhảy lên thoát khỏi cái hố là lấy được flag
## Hướng giải
-- Bật `DnSpy` và thay đổi 1 chút code nhé:
-- Thông thường để có thể xem và thay đổi dữ liệu của game, chúng ta thường tìm kiếm file `Assembly-Csharp.dll` vì đây là nơi lưu trữ hầu hết các data của game
-- Tìm đến hàm `Pitfall` của game:
![image](https://hackmd.io/_uploads/rkUT_OxMfl.png)
```clike=
if (other.CompareTag("Player"))
    {
        this.floor.SetActive(false);
        this.floor2.SetActive(false);
        GameObject[] array = this.pitObjects;
        for (int i = 0; i < array.Length; i++)
        {
            array[i].SetActive(true);
        }
        global::UnityEngine.Object.Destroy(this.flag);
    }
```
--> Một khi player chạm vào mặt đất thì `this.floor.SetActive(false)` và `this.floor2.SetActive(false)` . 2 câu lệnh dẫn đến mặt đất bị mất làm mình rớt xuống
-- Để sửa code nhấp chuột phải và chọn Edit class:
```clike=
private void OnTriggerEnter(Collider other)
{
    if (other.CompareTag("Player"))
    {
        
        return;
    }
 ``` 
-- File --> Save Module --> Ok để lưu file mới
--> Sau khi player chạm vào sàn thì flag cũng như sàn sẽ không biến mất 
![image](https://hackmd.io/_uploads/HybljOezGl.png)
--> Flag: GHCTF{You_should_play_basketball}
# Level 5

## Nhiệm vụ
-- Ở level này, chúng ta cần phải lấy flag trong cái base của boss
-- Nhưng mỗi khi đi vào chạm vùng đỏ(base boss) thì sẽ bị turret bắn chết
## Hướng giải
-- Bật `Dnspy` tìm hàm `LazerTurret` 
![image](https://hackmd.io/_uploads/rkT_T_gGGe.png)
-- Dễ thấy, 2 hàm `FireLazer` và `B` là 2 hàm khiến chúng ta bị bắn. Nhưng thật chất chỉ cần chỉnh hàm `FireLazer` là được
-- Sửa code:
```clike=
using System;
using MoreMountains.Tools;
using MoreMountains.TopDownEngine;
using UnityEngine;

// Token: 0x02000003 RID: 3
public partial class LazerTurret : MonoBehaviour, MMEventListener<MMGameEvent>, MMEventListenerBase
{
	// Token: 0x06000009 RID: 9 RVA: 0x000020E8 File Offset: 0x000002E8
	private void FireLazer()
	{
		return;// Tắt hết các hiệu ứng, không kill player
	}
}
```
![image](https://hackmd.io/_uploads/SJCFCOgffe.png)
--> Flag: GHCTF{OK_now_you_have_godmode}
# Level 6

## Nhiệm vụ
-- Giao 1 con cá cho thần mèo để có flag nhưng mà game lại cho mình 1 quả táo :v 
![image](https://hackmd.io/_uploads/BkWJZKlffg.png)
## Hướng giải
-- Bật `DnSpy` và tìm đến hàm `HasFish`
![image](https://hackmd.io/_uploads/HkNXZYlfGg.png)
```clike=
if (this.fishItems.Count > 0){
        this.completedFeedback.PlayFeedbacks();
        global::UnityEngine.Object.Instantiate<GameObject>(this.fishFlag, base.transform.position, Quaternion.identity);
        GameObject[] array = this.completionObjects;
        for (int i = 0; i < array.Length; i++)
        {
            array[i].SetActive(false);
        }
        array = this.antiCheatWarnings;
        for (int i = 0; i < array.Length; i++)
        {
            array[i].SetActive(true);
        }
        return;
        }
```
--> Dễ thấy nếu như chúng ta có hơn 1 con cá thì thần mèo sẻ nhả flag cho chúng ta :V
-- Sửa code:
```clike=
if (this.fishItems.Count >= 0)
```
![image](https://hackmd.io/_uploads/S1j8fYlzze.png)
--> Flag: GHCTF{Kitty_appreciates_the_fish_magic}
# Level 7

## Nhiệm vụ
-- Nhiệm vụ của chúng ta cần phải bắn hết tất cả chuột chũi trong vòng 10s, và điều này là không thể.
## Hướng giải
-- Bật `Dnspy` và tìm kiếm hàm `wackamole`, sau đó tìm hàm `CheckWin`
```clike=
private bool CheckWin()
	{
		Debug.Log(string.Format("{0}:{1}", this.spawnCount, this.killCount));
		if (this.spawnCount == this.killCount && this.spawnCount != 0)
		{
			this.completionFeedback.PlayFeedbacks();
			global::UnityEngine.Object.Instantiate<GameObject>(this.moleFlag, base.transform.position, Quaternion.identity);
			return true;
		}
		this.shootingFeedback.PlayFeedbacks();
		base.StartCoroutine(this.WaitDelay(0.5f));
		this.playerHealth.Kill();
		return false;
   }
```   
--> Điều kiện để chiến thắng là cần phải tiêu diệt đủ số chuột chũi đồng thời số chuột chũi sinh ra >0, vậy chỉ cần chỉnh lại số lượt tiêu diệt = số lượt spawn chuột chũi  là được
```clike=
private bool CheckWin()
	{
		Debug.Log(string.Format("{0}:{1}", this.spawnCount, this.killCount));
		this.spawnCount = 1;
		this.killCount = this.spawnCount;
		if (this.killCount == this.spawnCount && this.spawnCount != 0)
		{
			this.completionFeedback.PlayFeedbacks();
			global::UnityEngine.Object.Instantiate<GameObject>(this.moleFlag, base.transform.position, Quaternion.identity);
			return true;
		}
		this.shootingFeedback.PlayFeedbacks();
		base.StartCoroutine(this.WaitDelay(0.5f));
		return false;
	}
```
-- Sau đó chúng ta chỉ cần đứng yên 10s là sẽ qua màn
![image](https://hackmd.io/_uploads/r1YHwKlffx.png)
--> Flag: GHCTF{nice_shooting_tex}

# Level 8

## Nhiệm vụ
-- Cần phải mở 1 cái rương đang bị khóa để có flag
## Hướng giải
-- Bật `Dnspy`, tìm đến hàm `Lottery_check`
![image](https://hackmd.io/_uploads/rkJusYlzfe.png)
-- Nói ngắn gọn chỉ cần nhặt cho đến khi nào xuất hiện ra được flag, nhưng vì để pass level thì mình có thể sửa như sau:
```clike=
public void B(MMInventoryEvent inventoryEvent)
	{
		if (inventoryEvent.InventoryEventType != MMInventoryEventType.Pick)
		{
			return;
		}
		this.completionMMF.PlayFeedbacks();
	}
```
-- Điều này nghĩa là nếu mình nhặt bất kì item nào thì nó cũng sẽ coi nó như `flag` và bypass thành công

![image](https://hackmd.io/_uploads/rJAyntlGGg.png)

# Level 9

## Nhiệm vụ
-- Cần vượt qua màn đua và cách lượm hết xu trong vòng 10s để lấy flag
## Hướng giải
-- Bật `DnSpy` và tìm kiếm hàm `LazerTurretRace` ![image](https://hackmd.io/_uploads/Hy240YlMfl.png)
-- Để sau 10s không bị tiêu diệt thì cần xóa đi dòng `playerHealth.Kill()`
-- Sau đó đi lấy flag thôi
![image](https://hackmd.io/_uploads/ByMOAYgGfe.png)
--> Flag: GHCTF{zoom_zoom_money_maker}

# Level 10 (secret)
## Nhiệm vụ 
-- Đơn giản chỉ cần mở rương bị khóa với key thôi
## Hướng giải
-- Ban đầu mình tìm được 1 hàm `chest` nên sau khi giải hết 9 level thì mình mới bắt đầu hoài nghi
-- Lục 1 hồi thì mình tìm được hàm `fetch` chứa dấu hiệu khả nghi
![image](https://hackmd.io/_uploads/S1hhB9xGfg.png)
```clike=
public async Task<string> GetFlag(string levelSecret)
{
    await this.FetchSecret(levelSecret).AsTask(this);
    string text;
    if (this.encValue == "")
    {
        Debug.LogError("Error getting secret from server");
        text = "";
    }
    else
    {
        text = this.encValue;
    }
    return text;
}
```
-- Thấy rằng, hàm này nhận dữ liệu từ `levelSecret`,rồi gọi server lấy dữ liệu. Nếu lấy được thì trả về encValue, không được thì trả chuỗi rỗng ""
-- Mình suy nghĩ chắc là do `main menu` ẩn đi cái level cuối nên mình tàm luôn hàm `main menu` thử và thấy được hàm `Loadlevel`
--> Mình dùng GPT để hỗ trợ thì code đơn giản chỉnh như sau:
```
public void LoadLevel(int level)
	{
		if (level == 1){
			SceneManager.LoadScene(10);
		} else {
		SceneManager.LoadScene(level);
		}
	}
```
--> Nếu như trong mục `main menu` mình nhấn `level 1` thì lập tức sẽ chuyển mình qua màn `level10`, thử nhé


![image_300kb](https://hackmd.io/_uploads/S1Izu9eMGe.jpg)



-- Vào được map `secret ` luôn, bây giờ nhiệm vụ của mình là qua hàm `chest` để chỉnh xíu lấy flag nhé
![image](https://hackmd.io/_uploads/rJrHucgzGg.png)
-- bỏ đi `return` thôi là có được flag
![image](https://hackmd.io/_uploads/ry3qtcxzze.png)
--> Flag:GHCTF{well_played_hacker_gg}
		


