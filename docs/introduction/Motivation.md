# 動機

隨著對 JavaScript single-page 應用程式的要求越來愈複雜，JavaScript 程式碼高於以往**需要管理更多的 state**。這些 state 可能包括伺服器回應、快取的資料、和本地端建立但尚未保存到伺服器的資料。它也包括 UI 狀態，例如 active route、被選擇的 tab、是否要顯示 spinner 或 pagination 控制等等。

管理不斷變化的 state 是很困難的。如果一個 model 可以更新其他的 model，接著一個 view 可以更新一個 model，它更新了另一個 model，而這個，順帶的，可能造成另一個 view 被更新。到了某個時間點，你開始不再知道你的應用程式中發生了些什麼。**你不再控制 state 何時、為何、如何被更新。**當系統不透明且充滿不確定性，就很難去重現 bugs 或添加新的功能。

彷彿這樣還不夠糟糕，試想一些**在前端產品開發越來越普遍的新需求**，像是處理 optimistic updates、在伺服器上 rendering、在轉換 route 前抓取資料，等等。作為前端開發者我們發現自己被從來沒有處理過的複雜度給困住了，但[是時候放棄了嗎？](http://www.quirksmode.org/blog/archives/2015/07/stop_pushing_th.html)

這裡一大部份的複雜度來自於，事實上**我們混合了兩個概念**，它們都是人類的頭腦非常難去思考的概念：**變更和非同步。**我稱它們為 [Mentos and Coke](https://en.wikipedia.org/wiki/Diet_Coke_and_Mentos_eruption)。分開都好好的，但混在一起就變成一團糟。[React](http://facebook.github.io/react) 之類的 Libraries 試圖藉由移除非同步和直接的 DOM 操作，來在 view layer 解決這個問題。然而，React 留下了管理資料的 state 的部分讓你自己決定。

跟隨著 [Flux](http://facebook.github.io/flux)、[CQRS](http://martinfowler.com/bliki/CQRS.html) 和 [Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html) 的腳步，Redux 藉由強加某些限制在更新發生的方式和時機上，**試圖讓 state 的變化更有可預測性**。這些限制都反應在 Redux 的[三大原則](ThreePrinciples.md)中。
