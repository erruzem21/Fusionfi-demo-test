# Fusionfi-Demo-Test
Projeye burdan bakabilirsiniz 


https://x.com/FusionFiPro/status/1866426465279193382

## Testten önce nvm indirip kullanalım
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```
```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```
Nvm'nin doğru bir şekilde kurulup kurulmadığını kontrol etmek için:
```bash
nvm --version
```
Belirli bir Node.js sürümünü yüklemek ve seçmek için:
```bash
nvm install 21
nvm use 21
```
## Repoyu Klonlayalım
```bash
git clone https://github.com/permadao/ffp-demo.git

cd ffp-demo
```

## Teste Başlayalım
```bash
npm install aoffp
```

### 1. Yapılandırmayı ayarlayın
Test için birkaç yeni cüzdan oluşturun.
```bash
mkdir wallets
node ./generate.js
```
#### Wallets dizininin içindeki cüzdan dosyalarını yedekleyin. lazım olur :)

ortam değişkenleri ayarlayın
```bash
export $(cat .env.local | xargs)
```
### 2. Token airdrop'u alın
Test için $HELLO ve $KITTY token'larını ayarladık, bu token'ları şu şekilde alabilirsiniz.
```bash
node ./airdrop.js
```

Bu akış tamamlandığında bakiyeyi kontrol edebilirsiniz:
```bash
node ./balance.js --address=$WALLET1
```

Çıktı Böyledir
```bash
address: wBn738djx......................
$HELLO balance  100000000000000
$KITTY balance  100000000000000
```
Aynısı WALLET2 için de geçerli olacaktır

## Kullanım Örneği
Daha sonra ffp'nin emir defteri, AMM ve borç verme gibi kullanım durumlarını tanıtacağız.

### 1. Temel aracınızı oluşturun
```bash
node ./basic/create.js
```
```bash
Çıktı
wBn7-31aDtChhLfUk_eXNG9Nbafa_ghT29XRxk7osiM create agent: <YourAgent1>
ORHaLUrAiknTAq2Wszoyl6buJrd3MqDKLTF_2CggLtw create agent: <YourAgent2>
```

ortam değişkenleri ayarla
```bash
export $(cat .env.local | xargs)
```
### 2. Token'ı acentenize yatırın
```bash
node ./basic/deposit.js --walletN=1 --agentId=$AGENT1
```
Acentenizdeki bakiyeleri kontrol edecekseniz:
```bash
node ./balance.js --address=$AGENT1
```
Aynı işlemi ikinci ajan için de yapın.
```bash
node ./basic/deposit.js --walletN=2 --agentId=$AGENT2
```
```bash
node ./balance.js --address=$AGENT2
```
## Sipariş defteri
### 1. Sipariş defteri aracınızı oluşturun
```bash
node ./orderbook/create.js
```
```bash
Çıktı:
wBn7-31aDtChhLfUk_eXNG9Nbafa_ghT29XRxk7osiM create orderbook agent: <YourOrderBookAgent1>
ORHaLUrAiknTAq2Wszoyl6buJrd3MqDKLTF_2CggLtw create orderbook agent: <YourOrderBookAgent2>
```
ortam değişkenleri ayarla 
```bash
export $(cat .env.local | xargs)
```
### 2. Token'ı sipariş defteri acentenize yatırın
```bash
node ./orderbook/deposit.js --walletN=2 --agentId=$ORDERBOOKAGENT2
```
Acentenizdeki bakiyeleri kontrol edecekseniz:
```bash
node ./balance.js --address=$ORDERBOOKAGENT2
```
### 3. Sipariş verin
Yeni bir sipariş oluşturmak için bir ajanı kullanırız.
```bash
node ./orderbook/make.js --walletN=2 --agentId=$ORDERBOOKAGENT2
```
```bash
Çıktı:
openOrders {
  MVBggDjYkl3UxoHRZ2rO6ZLDcN4ax4Af6rehyYJ3CH0: {
    ID: 2600,
    AssetID: 'AttsQGi4xgSOTeHM6CNgEVxlrdZi4Y86LQCF__p4HUM',
    HolderAssetID: '0fLIp-xxRnQ8Nk-ruq8SBY8icaIvZMujnqCGU79fnM0',
    NoteID: 'MVBggDjYkl3UxoHRZ2rO6ZLDcN4ax4Af6rehyYJ3CH0',
    IssueDate: 1733297306962,
    HolderAmount: '3',
    Amount: '1',
    Status: 'Open',
    Price: 3,
    Issuer: 'RPFQd69SX2tbrtNBfVxzVSt9zQYk07WrHOCDhxDHu0o'
  }
}
```
Eğer herhangi bir sipariş çıktısı alamıyorsanız, sorgulama için aşağıdaki komutu kullanabilirsiniz.
```bash
node ./orderbook/query.js --agentId=$ORDERBOOKAGENT2
```
NoteID değişkenini yukardan bulup ayarlayın .
```bash
export NOTEID=<NoteID>
```
### 4. Sipariş almak için agent1'i kullanın
```bash
node ./basic/take.js --walletN=1 --agentId=$AGENT1 --noteId=$NOTEID
```
İşlem tamamlandıktan sonra her iki acentenin bakiyeleri güncellenmiş olur ve işlem tamamlanır.
```bash
node ./balance.js --address=$AGENT1
node ./balance.js --address=$ORDERBOOKAGENT2
```
## AMM
### 1. AMM aracınızı oluşturun
```bash
node ./amm/create.js
```
```bash
Çıktı:
wBn7-31aDtChhLfUk_eXNG9Nbafa_ghT29XRxk7osiM create amm agent: <YourAMMAgent1>
ORHaLUrAiknTAq2Wszoyl6buJrd3MqDKLTF_2CggLtw create amm agent: <YourAMMAgent2>
```
ortam değişkenleri ayarla
```bash
export $(cat .env.local | xargs)
```
### 2. Token'ı AMM acentenize yatırın    (burdan sonrasını diğer cüzdan için de ayrıca yapın (wallet1 )
AMM acentenize para yatırın
```bash
node ./amm/deposit.js --walletN=2 --agentId=$AMMAGENT2
```
Acentenizdeki bakiyeleri kontrol edecekseniz:
```bash
node ./balance.js --address=$AMMAGENT2
```
### 3. Havuz Ekle
yatırılan token'ı AMM likidite havuzunuza ekleyin
```bash
node ./amm/addPool.js --walletN=2 --agentId=$AMMAGENT2
```
Eğer acente havuzu bilgisini sorgulayacaksanız:
```bash
node ./amm/query.js --walletN=2 --agentId=$AMMAGENT2
```
```bash
Çıktı:
pools {
  "0fLIp-xxRnQ8Nk-ruq8SBY8icaIvZMujnqCGU79fnM0:AttsQGi4xgSOTeHM6CNgEVxlrdZi4Y86LQCF__p4HUM": {
    "py": "50",
    "algo": "UniswapV2",
    "fee": 30,
    "px": "50",
    "y": "AttsQGi4xgSOTeHM6CNgEVxlrdZi4Y86LQCF__p4HUM",
    "balances": {
      "0fLIp-xxRnQ8Nk-ruq8SBY8icaIvZMujnqCGU79fnM0": "50",
      "AttsQGi4xgSOTeHM6CNgEVxlrdZi4Y86LQCF__p4HUM": "50"
    },
    "x": "0fLIp-xxRnQ8Nk-ruq8SBY8icaIvZMujnqCGU79fnM0"
  }
}
```
### 4. AMM acentesinden bir AMM siparişi verin
```bash
node ./amm/request.js --walletN=2 --agentId=$AMMAGENT2
```
```bash
Çıktı:
order {
  "ID": 2601,
  "AssetID": "0fLIp-xxRnQ8Nk-ruq8SBY8icaIvZMujnqCGU79fnM0",
  "MakeTx": "zkmqIcN2KNOS38zgAq5AxTfnMeod7OaZUcy_SMOGJqE",
  "ExpireDate": 1733297855923,
  "HolderAssetID": "AttsQGi4xgSOTeHM6CNgEVxlrdZi4Y86LQCF__p4HUM",
  "NoteID": "14ec8heZ5m3XR6j9ZBOIUT-3lhq6qgloaz9ObIG-_PI",
  "IssueDate": 1733297765923,
  "HolderAmount": "5",
  "Amount": "4",
  "Status": "Open",
  "Price": 1.25,
  "Issuer": "xhgS6MeQ4qhYqP21ptsCSHY3m9faNsPs0ewRKB9jvwo"
}
```
NoteID yukadan bulup değişkenini ayarlayın . 
```bash
export NOTEID=<NoteID>
```
### 5. Bu siparişi acente aracılığıyla alın
```bash
node ./basic/take.js --walletN=1 --agentId=$AGENT1 --noteId=$NOTEID
```
İşlem tamamlandıktan sonra her iki acentenin bakiyeleri güncellenmiş olur ve işlem tamamlanır.
```bash
node ./balance.js --address=$AGENT1
```
```bash
node ./amm/query.js --walletN=2 --agentId=$AMMAGENT2
```
### 6. Emir defteri emri ve AMM acentesi ile arbitraj
```bash
node ./arbitrage.js --agentId=$AGENT1 --orderbookAgentId=$ORDERBOOKAGENT2 --ammAgentId=$AMMAGENT2
```

## https://www.ao.link/   den cüzdanları aratarak neler yaptığınzıı görebilirsiniz.



