# 簡易版C語言Interpreter

## 前言 

![夏老大形象照](/picture/HsiaBigWig.png)

*以下取自夏老大的名言*

> 歡迎來到資工系

> 以後你看別人(或是自己)的程式，一定心裡會有OS──幹

> 你唯一不能騙的人就是你自己(邏輯)

> 寫完PL後，你會感謝我的

自從聽到歡迎來到資工系，就打開了通往古老(系統真的很舊)的地獄大門CAL，雖然前期的猜數字對有點基礎的人來說有點過於簡單且惱人(猜錯很多次QQ)，但後期的一題一題的作業，也讓我對程式語言有更加嚴謹的認識。當初寫CAL覺得程式太小，就沒有寫很多題解和註解，但這次的PL Project動輒千行程式，為了讓未來的自己看得懂程式碼，所以這次的程式碼會有盡量詳細的題解和註解。  
-2024/2/24 Joe

  
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
    - *第一個ID出現常見問題*
        由於讀取到第一個ID時，無法判斷是變數宣告還是運算式，所以等到讀到下個token時，就必須檢查是否為宣告或是運算，如果是宣告就將變數存到變數表中，如果是運算就需要檢查變數表中是否有此變數，如果沒有就報錯。(後續Project可能也有類似隱測)
    - *為什麼真的建出一顆AST，而不是直接計算?*  
        為了後面Project鋪路，在進行程式的運算時，預想到可能有statement(e.g. while if)問題(以同樣tree結構可建立而成)，並且也較為直觀。
    - *為什麼要try catch?*  
        因為只要出現錯誤就會直接flush buffer，並且輸出錯誤訊息，而不會等到讀取到';'才輸出錯誤訊息，可以使程式間耦合性降低，並且可以更快速的找到錯誤點。
    - *為什麼要強調一定要製作Interact的程式?*  
        因為在實際使用程式時，不會一次輸入所有的運算式，而是一次一個token的Error check，所以必須要能及時的報錯。

- 執行範例圖示:  
    ![Project1EXE](/picture/project/project1.png)
    ![Project1GIF](/picture/project/Project1GIF.gif)
---
> 「咱大家來爬偉大的山 看彼美麗的海岸」 - 《關閉太陽》 **血肉果汁機**
![BOSS](/picture/Boss.png)

做完Project1後，聽到助教建議使用java，可謂心涼大半，但後來想想寫程式本來就是一個解決問題的過程，所以我暫時不會轉換程式語言，而是會繼續先前用C++寫的Project1架構做延伸。我想這就是寫程式的固執與浪漫Na~  雖然大家都說Project 1與234差很多，但我認為對於我所認知的AST架構來說，只是文法上面的概念些微不同，本質還是一樣的。  
-2024/3/7 Joe

---
#### Project2
 跳過
---
#### Project3
- 程式運作流程(與Project1大致相同): 
    1. 讀取TestNum
    2. 將字串分割成token，並判斷token的類型
    3. 每切成功一個token就放進parser中，當parser長成一個完整的AST樹時，就進行模擬
    4. 模擬完成後，依照指令類型輸出結果例如: Statement executed ...

- 提醒:
    1. if或while以及do內的宣告會變成區域變數，所以在if內宣告的變數，不會影響到外面的變數
    2. 變數當作一種 **class** ，因為有可能會轉型例如 陣列轉單個
    3. 可以製作 **假執行**，因為有可能遇到還未長成一棵樹就需檢查區域變數的存在，例如while內的區域變數，簡單來說在生成樹時，假裝宣告這個數
    4. float運算大數可能會變成科學符號

- Scope概念  
在寫Project1 的時候就很好奇各個變數可視範圍的邏輯，也不太想參照課本的ID table，於是依照自己的想像力做出來以Scope為主體的Object，跟迴圈一樣可以洋蔥式包覆，以及擁有向外搜尋的功能  
![Scope概念圖](/picture/LogicConcept/ScopeConcept.jpg)

- AST
目的是為了簡化運算流程，哪個部分出狀況改那個部分就可以，實際架構依個人喜好設定即可
![AST概念圖](/picture/Xmind/AST.png)

- 設問:  
    - *為什麼每次運算都需回傳ID?*  
        模擬Python的方式(萬物皆Object)，可以延伸至回傳array function等

- 執行範例圖示:  
    ![Project3EXE](/picture/project/Project3.png)
    ![Project3GIF](/picture/project/Project3GIF.gif)

---
#### Project4

- 程式運作流程(與Project3一模一樣): 
    1. 讀取TestNum
    2. 將字串分割成token，並判斷token的類型
    3. 每切成功一個token就放進parser中，當parser長成一個完整的AST樹時，就進行模擬
    4. 模擬完成後，依照指令類型輸出結果例如: Statement executed ...

- 提醒:
    1. function傳址功能也要設計成洋蔥式
    2. function內的變數不會影響到外面的變數
    3. 在定義時就必須檢查所有error，例如變數是否已宣告過，function是否已定義過等等

- FunctionCall概念  
由於我的架構是Scope包覆為主，所以要特別注意在function call時不能取用到同名還未被宣告的變數，所以增加了buffer的功能，當function call時，會將新宣告的變數存到buffer
，等到function call 所有變數宣告完後，再將buffer的變數存到Scope中，這樣就可以避免變數名稱衝突的問題
![FunctionCall概念圖](/picture/LogicConcept/FunctionCallConcept.jpg)

- 設問:
    - *如何呼叫function?*  
        由於在一開始做Project1的時候就有想過function的概念，所以對我來說不管是整個function或是整個程式都可以用節點來表示，只要將function節點存在vector中，當遇到function call時，就可以直接呼叫function節點

    - *如何取址?*  
        採用python的概念，任何identifer都是Object，所以只要將Object的指標傳遞給欲取址的identifer即可
    
    - *如何避免變數名稱衝突?*  
        透過buffer的概念，當function call時，會將新宣告的變數存到buffer，等到function call 所有變數宣告完後，再將buffer的變數存到Scope中，這樣就可以避免變數名稱衝突的問題

- 執行範例圖示:  
    ![Project4EXE](/picture/project/Project4.png)
    ![Project4GIF](/picture/project/Project4GIF.gif)

---

## PAL Project紀念截圖
因為有可能是最後一屆使用PAL Project，所以特此紀念

![ProblemSolve](/picture/project/CongratulationProblemSolve.png)

![Project 3 and 4](/picture/project/34system.png)

![ProjectSolve](/picture/project/CongratulationProjectSolve.png)

---

![BrightFuture](/picture/BrightFurture.png)

## 結語
雖然這次PL Project花了很多時間，但也學到了很多東西，例如AST、Scope、FunctionCall等等，感謝夏老大的教導，讓我能夠更加了解程式語言的本質，也感謝助教的提點，讓我能夠避開PAL的雷點，也感謝同學的討論，每次的交流都讓我學到不同的創意，也感謝自己的堅持，讓我能夠更加了解程式語言之美。雖然到了大三大家都很忙，不管有沒有寫完PL的各位都祝福大家能達成心中的理想。  
-2024/3/28 Joe

