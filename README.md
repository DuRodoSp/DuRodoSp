
$c[Coloque o nome da sua variavel aí em baixo]

$var[name;moeda]

$c[/==================/]
$var[valor;$replaceText[$replaceText[$replaceText[$message[1];k;000];m;000000];all;$getVar[$var[name];$authorID]]]
$argsCheck[>1;❌ | <@$authorID>, **Digite o valor que irá apostar!** Use: ``y.batal <valor>]
$onlyIf[$isNumber[$var[valor]]==true;**O valor de aposta deve conter apenas números!**]
$onlyIf[$round[$var[valor]]>0;<@$authorID>, Digite apenas valores positivos!]
$onlyIf[$getVar[$var[name];$authorID]>=$var[valor];❌ | <@$authorID>, **Você nem tem esta quantia para apostar!**]
$if[$or[$message[2]==;$isNumber[$message[2]]==false]==true] $var[slots;5] $else $var[slots;$round[$message[2]]] $endif
$onlyIf[$and[$var[slots]>=1;$var[slots]<=20]==true; **O número de participantes deve ser um número maior que 1 e menor que 20**.]
$globalCooldown[1m;⚔️ | **Você tem uma batalha pendente, aguarde ela finalizar para iniciar outra!**]
$title[⚔️ | Batalha]
$description[$username iniciou uma batalha valendo **$numberSeparator[$round[$var[valor]]]** a batalha será iniciada automaticamente em 60 segundos caso haja participantes suficientes.

Valor atual: $numberSeparator[$round[$var[valor]]]

**[ 🗡️ \] | Participantes  (1/$var[slots])**
<@$authorID>
]
$color[ffffff]
$setVar[bat;$var[valor]/$authorID;$authorID]
$setVar[$var[name];$sub[$getVar[$var[name];$authorID];$var[valor]];$authorID]
$addButton[no;batalha-$authorID-$var[slots]-$var[valor];Batalhar;secondary;no;⚔️]
$addButton[no;endbattle-$authorID-$var[slots]-$var[valor];Finalizar;primary;no;🍀]
$async[oi]
$replyIn[60s]
$removeButtons
$onlyIf[$getVar[bat;$authorID]!=;]
$nomention
$textSplit[$getVar[bat;$authorID];/] $var[valor;$splitText[1]] $removeSplitTextElement[1]

$if[$sum[$getTextSplitLength;1]<=2] $sendMessage[❌ - <@$authorID> A batalha não teve usuários suficientes! (1/$var[slots])] $setVar[$var[name];$sum[$getVar[$var[name];$authorID];$var[valor]];$authorID] $setVar[bat;;$authorID]$else
$try
$var[winner;$splitText[$random[0;$sum[$getTextSplitLength;1]]]]
$catch $var[winner;$authorID]

$endtry
$sendMessage[✅ | <@$var[winner]> Venceu a batalha de <@$authorID> que estava valendo **$numberSeparator[$var[valor]]**, contra ($getTextSplitLength) participantes! ]
$setVar[$var[name];$sum[$getVar[$var[name];$var[winner]];$var[valor]];$var[winner]]
$setVar[bat;;$authorID]
$endif
$endasync

$onInteraction



$if[$textSplit[$customID;-]$splitText[1]==batalha]
$c[Coloque o nome da sua variavel aí em baixo]

$var[name;moeda]

$c[/==================/]
$nomention
$onlyIf[$getVar[bat;$splitText[2]]!=;]
$var[slots;$splitText[3]]
$var[autor;$splitText[2]]
$var[valorini;$splitText[4]]
$if[$checkContains[$getVar[bat;$var[autor]];$authorID]==true] $removeButtons $ephemeral ❌ | Você já entrou nesta batalha! 
$elseif[$textSplit[$getVar[bat;$var[autor]];/]$removeSplitTextElement[1]$getTextSplitLength>=$var[slots]] $ephemeral $removeButtons A batalha está cheia! 
$elseif[$textSplit[$getVar[bat;$var[autor]];/]$getVar[$var[name];$authorID]<$var[valorini]] $removeButtons $ephemeral Você não tem a quantidade necessária para entrar na batalha! $else
$var[a;$getVar[bat;$var[autor]]]
$textSplit[$var[a];/]
$var[valor;$splitText[1]]
$setVar[$var[name];$sub[$getVar[$var[name];$authorID];$var[valorini]];$authorID]

$setVar[bat;$replaceText[$joinSplitText[/]/$authorID;$var[valor]/;;-1];$var[autor]]
$textSplit[$getVar[bat;$var[autor]];/]
$var[valor;$multi[$getTextSplitLength;$var[valorini]]]
$setVar[bat;$var[valor]/$getVar[bat;$var[autor]];$var[autor]]

$title[⚔️ | Batalha]
$description[$username[$var[autor]] iniciou uma batalha valendo **$numberSeparator[$var[valorini]]** a batalha será iniciada automaticamente em 60 segundos caso haja participantes suficientes.

Valor atual: $numberSeparator[$var[valor]]

**[ 🗡️ \] | Participantes  ($getTextSplitLength/$var[slots])**
$trimSpace[$replaceText[$textSplit[<@$textSplit[$getVar[bat;$var[autor]];/]$removeSplitTextElement[1]$joinSplitText[><@]>;>] $joinSplitText[>
];<@$var[valor]>;;-1]]
]
$color[ffffff]
$endif
$endif

$onInteraction


$var[name;moeda]

$textSplit[$customID;-]
$if[$and[$splitText[1]==endbattle;$splitText[2]==$authorID]==true]

$var[slots;$splitText[3]]
$var[valor;$splitText[4]]
$onlyIf[$getVar[bat;$authorID]!=;]
$nomention
$textSplit[$getVar[bat;$authorID];/] $var[valor;$splitText[1]] $removeSplitTextElement[1]

$if[$sum[$getTextSplitLength;1]<=2] $sendMessage[❌ - <@$authorID> A batalha não teve usuários suficientes! (1/$var[slots])] $setVar[$var[name];$sum[$getVar[$var[name];$authorID];$var[valor]];$authorID] $setVar[bat;;$authorID]$else
$try
$var[winner;$splitText[$random[0;$sum[$getTextSplitLength;1]]]]
$catch $var[winner;$authorID]

$endtry
$sendMessage[✅ | <@$var[winner]> Venceu a batalha de <@$authorID> que estava valendo **$numberSeparator[$var[valor]]**, contra ($getTextSplitLength) participantes! ]
$setVar[$var[name];$sum[$getVar[$var[name];$var[winner]];$var[valor]];$var[winner]]
$setVar[bat;;$authorID]
$endif
$endif
