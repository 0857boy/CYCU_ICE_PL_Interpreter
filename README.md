# 簡易版C語言Interpreter

## 前言 

![夏老大形象照](/picture/夏老大形象照.png)

*以下取自夏老大的名言*

> 歡迎來到資工系

> 以後你看別人(或是自己)的程式，一定心裡會有OS──幹

> 你唯一不能騙的人就是你自己(邏輯)

> 寫完PL後，你會感謝我的

自從聽到歡迎來到資工系，就打開了通往古老(系統真的很舊)的地獄大門CAL，雖然前期的猜數字對有點基礎的人來說有點過於簡單且惱人(猜錯很多次QQ)，但後期的一題一題的作業，也讓我對程式語言有更加嚴謹的認識。當初寫CAL覺得程式太小，就沒有寫很多題解和註解，但這次的PL Project動輒千行程式，為了讓未來的自己看得懂程式碼，所以這次的程式碼會有盡量詳細的題解和註解。-2024/2/24 Joe

  
## 簡介
此程式為中原大學三下PL(程式語言)project 占分70% 分為 project1~4  
Project1: 分割token及基礎的運算能力及變數宣告 (15%)  
Project2: OurC語言文法的check(不需要實際運算) (25%)  
Project3: 2的進階 除了確認文法的正確性還要加上運算能力及輸出答案  (17.5%)  
Porject4: 3的進階 要加入Call function的能力    (12.5%)

## 使用工具
- 環境:   Visual Studio Code
- 程式語言: C++
- AI輔助工具: ChatGPT3.5、Github Copilot、coze(內含ChatGPT4.0 Turbo)
- 心智圖: XMind


### 題目

#### Project1

![Project1Xmind](/picture/Xmind/Project1.png)

- 程式運作流程: 
    1. 讀取TestNum
    2. 將字串分割成token，並判斷token的類型
    3. 每切成功一個token就放進parser中，當parser長成一個完整的運算式時，就進行運算
    4. 運算完後，將結果放進變數中並且輸出

- 提醒:
    1. 必須實作能Interact的程式 舉例來說當讀取到 "abc + \n"時，應該要及時報錯 Undefined identifier: abc 而不是等到讀取到 ';' 才輸出
    2. 在parse時可以順便生成**AST**(Abstrac Syntax Tree) ， 計算部分交給其他class處理
    3. 可以使用try catch來處理錯誤

- 設問:  
    - *為什麼真的建出一顆AST，而不是直接計算?*  
        為了後面Project鋪路，在進行程式的運算時，預想到可能有statement(e.g. while if)問題(以同樣tree結構可建立而成)，並且也較為直觀。
    - *為什麼要try catch?*  
        因為只要出現錯誤就會直接flush buffer，並且輸出錯誤訊息，而不會等到讀取到';'才輸出錯誤訊息，可以使程式間耦合性降低，並且可以更快速的找到錯誤點。
    - *為什麼要用Interact的程式?*  
        因為在實際使用程式時，不會一次輸入所有的運算式，而是一次一個token的Error check，所以必須要能及時的報錯。

- 執行範例圖示:
    ![Project1EXE](/picture/project/project1.png)

#### Project2

#### Project3

#### Project4





