##CM3D2.CycloneX10.Plugin

カスタムメイド3D2に電動オナホ「[A10CycloneSA][]」を
連動させるプラグインです。
  
現状夜伽のみ対応  

F10キーを押すことにより実行された振動のパターンと強さをデバッグ情報として画面左上に表示されます。  
F11キーを押すことによりA10Cycloneのデバッグ用の再生ウィンドウが表示されます。  

ウィンドウ上にあるXML再読み込みボタンを押すことにより、ゲームを再起動する事なく  
各種振動設定XMLファイルを再読み込みして更新をする事ができます。  

##不都合など
コマンド取得フックなどの関係で現状幾つかのプラグインと同居できない問題があります。

現状同居できないプラグイン  
・AddYotogiSlider

・CM3D2.CycloneX10.Plugin

COMポートを使用して通信していますが、今のところ接続先の機器名称を取得する方法が
見つかっていないため初期化時は決め打ちでCOM4に通信を行っています。(作者のPC環境はCOM4で認識されるため)
BTドングルなどを接続している等の関係でCOM番号が異なっている場合、
コントロールパネル > デバイスマネージャ を開いて表示される「ポート(COMとLPT)」を参照し、
Vorze_USB(COMXX) と記載されているCOM番号を控えておき、
夜伽画面に遷移した後に後述のデバッグ画面を表示して「切断」後、控えておいたCOM番号に再接続を行ってください。

##開発・動作環境
カスタムメイド3D2	Ver1.43.1
  
##導入方法

###前提条件  : **Sybarys** 又は **UnityInjector** が導入済みであること。  

[![ダウンロードボタン][img_download]][master zip]を押してzipファイルをダウンロード。   
解凍後CM3D2.A10Cyclone.Plugin.dllファイルを./Sybaris/Plugins/UnityInjector/フォルダに入れる。

特定のフォーマットで記載したxmlファイルを指定のディレクトリに入れる事により動作します。

./Sybaris/Plugins/UnityInjector/Config/A10CycloneXml/

※フォルダが無い場合は自動で生成されます。
     
##注意書き

個人で楽しむ為の非公式Modです。  
転載・再配布・改変・改変物配布等は「KISS」又は「Vorze」に迷惑のかからぬ様、  
各自の判断・責任の下で行って下さい。  
ハードが絡むプラグインの為、よくテストをしてからご使用ください。  ***(もげても責任取れません)***

##設定ファイルについて
全夜伽グループ名+コマンドを表記したxmlファイルをZipで同封しました。   
各夜伽コマンドの設定はそのZip内のファイルを元に加筆、修正をしてください。  

***○ EditInformation (任意)***  
編集者、編集日、コメントなど 項目自体省略可能

```
<EditInformation>
    <EditName>編集者名</EditName>         
    <TimeStamp>タイムスタンプ</TimeStamp>
    <Comment>自由に記載OK</Comment>
  </EditInformation>
```

***○ 識別名 (必須)***  
振動させるタイプを定義する項目です。  
必要な分だけ作成をしてください。
  
・Lv0		振動の強さ(0-127の数字) 興奮値 0以下  
・Lv1		振動の強さ(0-127の数字) 興奮値 0-100   
・Lv2		振動の強さ(0-127の数字) 興奮値 100-200   
・Lv3		振動の強さ(0-127の数字) 興奮値 200以上   
・識別名	識別する名前  ↓の例でいう「STOP」や「PreSet0」、「PreSet1」

```
  <LevelList>
    <LevelItem Lv0="0" Lv1="0" Lv2="0" Lv3="0">STOP</LevelItem>
    <LevelItem Lv0="1" Lv1="1" Lv2="3" Lv3="5">PreSet0</LevelItem> 
    <LevelItem Lv0="1" Lv1="2" Lv2="3" Lv3="4">PreSet1</LevelItem> 
    <LevelItem Lv0="1" Lv1="3" Lv2="5" Lv3="7">PreSet2</LevelItem> 
    <LevelItem Lv0="1" Lv1="4" Lv2="7" Lv3="9">PreSet3</LevelItem>
  </LevelList>
```

***○ YotogiCXConfig (必須)***  
振動させるデータを定義する項目
GroupNameはゲーム内の夜伽グループ名と同じ物を入力する事
```
<YotogiCXConfig>
    <GroupName>処女喪失セックス</GroupName>
    <YotogiList>
	    <!--ここは次項を参照-->
        <YotogiItem>～～～</YotogiItem>
		<YotogiItem>～～～</YotogiItem>
    </YotogiList>
<YotogiCXConfig>
```

***○ YotogiItem (必須)***  
振動させる夜伽グループの中のコマンドを定義する項目
Yotogi_Nameはゲーム内の夜伽コマンドと同じ物を入力する事

< Insert >は将来の為に項目だけ作成、省略可能  
挿入時のアニメーションを定義する予定

```
      <YotogiItem>
        <Yotogi_Name>責める</Yotogi_Name>
        <ControlData>
          <Control />
          <Control />
          <Control />
        </ControlData>
      </YotogiItem>
```
***○ Control (必須)***  
振動のパターンを並べて細かい動きを設定できます。

Controlで使用できる属性一覧  
・Pattern	：指定した振動パターンにする(0=正転、1=逆転)  
・Level		：指定した振動レベルにする(0=停止)  
・LevelName	：興奮値による振動レベルを適用(LevelListの識別名と一致させる事)  
・Delay		：実行前に指定したDelay(秒)待機をする  
・Time		：実行後に指定したTime(秒)待機をする  
・Insert	：trueの場合非挿入→挿入時のみ実行をする  
・Personal	：メイドの性格が一致する場合に実行をする  
 LevelとLevelNameがそれぞれ定義されていた場合はLevelNameを優先  

○停止させるには  
 < Control Level="0" /> =>停止と同等  

○Insertフラグ(挿入時のみ実行する)
```
<Normal>
　<!--挿入時のみ実行-->
  <Control Pattern="0" Level="0" Time="0.8" Insert="true" />   停止 0.8秒後に次項目
  <Control Pattern="0" Level="1" Time="0.2" Insert="true" />   パターン 0 振動レベル 1 で0.2秒後に次項目　　
  <Control Pattern="0" Level="0" Time="0.5" Insert="true" />   停止 0.5秒後に次項目
  <Control Pattern="0" Level="1" Time="1.0" Insert="true" />   パターン 0 振動レベル 1 で1.0秒後に次項目
  <Control Pattern="0" Level="0" Time="0.5" Insert="true" />   停止 0.5秒後に次項目
  <Control Pattern="3" Level="1" Time="1.0" Insert="true" /> 　パターン 3 振動レベル 1 で1.0秒後に次項目
　<!--通常責め時-->
  <Control Pattern="3" LvName="PreSet3" />     　パターン 3 興奮値のレベルで振動を続ける
</Normal>
```
○Personal設定(メイドの性格に応じたパターン設定)
```
<Normal>
  <!--おねだり中(純真妹系)-->
  <Control Pattern="4" Level="3" Time="15.0" Personal="Pure" />
  <Control Pattern="8" Level="6" Time="30.0" Personal="Pure" />
  <!--おねだり中(クール)-->
  <Control Pattern="4" Level="3" Time="15.0" Personal="Cool" />
  <Control Pattern="8" Level="6" Time="30.0" Personal="Cool" />
  <!--おねだり中(プライド)-->
  <Control Pattern="4" Level="3" Time="15.0" Personal="Pride" />
  <Control Pattern="8" Level="6" Time="30.0" Personal="Pride" />
  <!--中出しサンプル-->
  <Control Pattern="0" Level="0" Time="0.8" />
  <Control Pattern="0" Level="3" Time="0.2" />
</Normal>
```

##更新履歴

###0.0.0.1 [2017/01/10]
[CM3D2.CycloneX10.plugin][]を元にA10Cycloneに対応。
上記に応じてxmlファイルの振動パターン、振動レベルの範囲を変更.

##課題などメモ  
・夜伽以外で対応可能か？(VIP夜伽など)  
　　→VIP夜伽のシーン番号は？  
　　→会話IDなど再生するタイミングが取得できれば実現できそう？  

##謝辞
本プラグインを開発するにあたり、[CM3D2.CycloneX10.Plugin][]を流用させて頂きました。
作者様にこの場を借りてお礼申し上げます。

[A10CycloneSA]: http://www.vorze.jp/a10cyclonesa/ "A10CycloneSA"
[CM3D2.CycloneX10.Plugin]: https://github.com/sirogane/CM3D2.CycloneX10.plugin/ "CM3D2.AddModsSlider.Plugin/"
[master zip]: https://github.com/obtdai/CM3D2.CycloneX10.plugin/archive/master.zip "master zip"
[img_download]: http://i.imgur.com/byav3Uf.png "ダウンロードボタン"
