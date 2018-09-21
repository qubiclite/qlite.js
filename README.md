# qlite.js

## ABOUT

This library allows you to connect to existing ql-nodes and use their API to interact with the Qubic Lite protocol.

## USE

### 1. download `qlite.js` and put it into your project's directory

### 2. include the library into your .html file

```html
<!DOCTYPE html>
<html>
<head>
  ...
  <script type="text/javascript" src="../js/qlite.js"></script>
</head>
<body> ... </body>
</html>
```

### 3. initialize a new qlite instance in your javascript application

```js
const QLITE = new Qlite('https://qlnode.org:17733');
```

### 4. send requests

```js
var my_qubic = "GAPGBVIBDKTGZ9BVLCZYWPZAFMIXBDLCUTXOC9NEJ9HGDKZYGRPQVIHMZXRXCDLZIFXGECZBFSTTNA999";

QLITE.qubic_read(function(res, err) {

  if(err) // something went wrong
    console.log(err);
  else // everything is fine
    console.log("qubic code: " + res['code']);
  
}, my_qubic);
```
# API DOCUMENTATION

* App
    * [app_list](#app_list)
    * [app_install](#app_install)
    * [app_uninstall](#app_uninstall)
* Qubic
    * [qubic_read](#qubic_read)
    * [qubic_list](#qubic_list)
    * [qubic_create](#qubic_create)
    * [qubic_delete](#qubic_delete)
    * [qubic_list_applications](#qubic_list_applications)
    * [qubic_assemble](#qubic_assemble)
    * [qubic_test](#qubic_test)
* IAM
    * [iam_create](#iam_create)
    * [iam_delete](#iam_delete)
    * [iam_list](#iam_list)
    * [iam_write](#iam_write)
    * [iam_read](#iam_read)
* General
    * [node_info](#node_info)
    * [change_node](#change_node)
    * [fetch_epoch](#fetch_epoch)
    * [export](#export)
    * [import](#import)
    * [qubic_consensus](#qubic_consensus)
* Oracle
    * [oracle_create](#oracle_create)
    * [oracle_delete](#oracle_delete)
    * [oracle_list](#oracle_list)
    * [oracle_pause](#oracle_pause)
    * [oracle_restart](#oracle_restart)
***
## FUNCTIONS

### `node_info()`
Gives details about this ql-node.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
#### example call
```js
QLITE.node_info(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
});
```
#### example response
```json
{"duration":42,"iota_node":"https://nodes.devnet.thetangle.org:443","success":true,"testnet":true,"version":"0.5.0"}
```
***
### `change_node()`
Changes the IOTA full node used to interact with the tangle.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `node_address`  | `string (nodeaddress)` | address of any IOTA full node api (mainnet or testnet, depending on which ql-nodes you want to be able to interact with)
| `mwm`  (opt.) | `int (integer{9-14})` | min weight magnitude: use 9 when connecting to a testnet node, otherwise use 14
#### example call
```js
QLITE.change_node(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'https://node.example.org:14265', 14);
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `fetch_epoch()`
Determines the quorum based result (consensus) of a qubic's epoch.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `qubic`  | `string (trytes{81})` | qubic to fetch from
| `epoch`  | `int (integer{0-2147483647})` | epoch to fetch
| `epoch_max`  (opt.) | `int (integer{-1-2147483647})` | if used will fetch all epochs from 'epoch' up to this value
#### example call
```js
QLITE.fetch_epoch(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'SDTMJ9ZYYTLONLFNFGQVVAY99CHQ9TKOTFIZXENKZHOPQTCSVWGGSBNH9PYHQWPUSAOXWT9ZRS9H9SADG', 4, 7);
```
#### example response
```json
{"last_completed_epoch":815,"duration":42,"fetched_epochs":[{"result":"49","quorum":2,"epoch":7,"quorum_max":3},{"quorum":1,"epoch":8,"quorum_max":3},{"result":"81","quorum":2,"epoch":9,"quorum_max":3}],"success":true}
```
***
### `export()`
Transforms an entity (iam stream, qubic or oracle) into a string that can be imported again.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `id`  | `string (trytes{81})` | id of the entity to export
#### example call
```js
QLITE.export(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'CZLOLNIRWAONKQLQMFAQMRDMONALUXECDFPUSGPSMWUDFMVVIDNVOQPEUHQOZUTQEOYLSUGXIVLRRA9YP');
```
#### example response
```json
{"duration":42,"success":true,"export":"q_ISDWNHXQLFTRTUPSTGFA9ONZGQN9TUICPOGQISGIBQAQMYCXRKEFQJBTMHFPKURZDYNTAVIJFDPXEC999_GUACEZHWFAEZEYGUACEZGQFEFFGOAGHSDAHCFCEZGUACEZGDFAABABEYEVJVIDABGBJLFQGNICDRHUBCGSEEDWDZEOFPCDICHGEHHOEYCPGCHJAACCIBGKIZHPINHKGGIBETIJHHANIIESCLCRENCGGUEOCXBBIFJCDJABHFAAGBGYJFEKIWIQCDJBAZIABLBKBFBFEAFCJRFOGGCOHZCHBPDJEWCDCSFZEQHFIHDZCSBOBMFTFNFCETADEODFCRGCCPFAGZIEFRIKFUARGWEOJLELBUGPIRDJGOEHEKGGFBFXBDDDHSEZCTFAFTEYAXIQIAAPFTGHFJCYBYASCFACBIEDAEFJEIIIGAENFAABABEYEPDTBGAFDIBBHHDQCXCIBRIMHACEIHCFJPAUBVCHESHEECACERIHHWFJHHFFACIXIBIJIHAOCGDGIJHZDYJHFFFOABAACAHTFUJHGHEAHWGMFUFRCDDBFHGWAMCUBMDTHGFUJQALIEJSANGMDSBJBUGCGPBZBMJLARJEBJJVFJESGFGZISEJETISJQEZGIHFCYBKEJCKBOIBAQAJBOADDRDTIKDXBFFEASALIWIOAAJRIFGJIUEZHWHFEWDBHTGOFCFVFAFTEYAQDUEWCCEDFHCNHWFQBRGRGCCVDEHHDGERHLFGAN"}
```
***
### `import()`
Imports a once exported entity (iam stream, qubic or oracle) encoded by a string.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `encoded`  | `string (string)` | the code you received from command 'export' (starts with 'i_', 'o_' or 'q_')
#### example call
```js
QLITE.import(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'q_HZGULHJSZNDWPTOCXDYYKMKXCCKCHPORELEBZLBQRWHQNBMNAHBGWQYD9WRVHFKRQRXUXLXORJEPTN999_GUACEZHWFAEZEYGUACEZGQFEFFGOAGHSDAHCFCEZGUACEZGDFAABABEYEVJVIDABGBJLFQGNICDRHUBCGSEEDWDZEOFPCDICHGEHHOEYCPGCHJAACCIBGKIZHPINHKGGIBETIJHHANIIESCLCRENCGGUEOCXBBIFJCDJABHFAAGBGYJFEKIWIQCDJBAZIABLBKBFBFEAFCJRFOGGCOHZCHBPDJEWCDCSFZEQHFIHDZCSBOBMFTFNFCETADEODFCRGCCPFAGZIEFRIKFUARGWEOJLELBUGPIRDJGOEHEKGGFBFXBDDDHSEZCTFAFTEYAXIQIAAPFTGHFJCYBYASCFACBIEDAEFJEIIIGAENFAABABEYEPDTBGAFDIBBHHDQCXCIBRIMHACEIHCFJPAUBVCHESHEECACERIHHWFJHHFFACIXIBIJIHAOCGDGIJHZDYJHFFFOABAACAHTFUJHGHEAHWGMFUFRCDDBFHGWAMCUBMDTHGFUJQALIEJSANGMDSBJBUGCGPBZBMJLARJEBJJVFJESGFGZISEJETISJQEZGIHFCYBKEJCKBOIBAQAJBOADDRDTIKDXBFFEASALIWIOAAJRIFGJIUEZHWHFEWDBHTGOFCFVFAFTEYAHDRGKJTIPFGBHHIGIDNAABHFEEEGEAYJIIVFKDD');
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `qubic_read()`
Reads the specification of any qubic, thus allows the user to analyze that qubic.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `qubic`  | `string (trytes{81})` | id of the qubic to read
#### example call
```js
QLITE.qubic_read(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'XZTUUANZCHSSPROSIRCFPKBDSUPNCBPDADBIJYXUZHSPOPGPNANOMUUSPTDXRK9XSPXRLWPWSEJSHUCXV');
```
#### example response
```json
{"assembly_list":["B9PCXT9GZYXXTIJ9NWUWZGJCK9FKAHQFDGOWP9NVYGBKK9STYZ9ABZHKRZTWPGPLENGRHGPPONIJWZWKW"],"duration":42,"code":"return(epoch^2);","hash_period_duration":20,"result_period_duration":10,"success":true,"runtime_limit":10,"execution_start":1537543279,"id":"9XXRJJGTWTXWHBJPHCZUQJXOUWPIZAXTMJTIPNRAOIXTXSWZPHEKMGWNQKRBOSDDNCNDJKGRUNDUKY999","version":"ql-0.5.0"}
```
***
### `qubic_list()`
Lists all qubics stored in the persistence.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
#### example call
```js
QLITE.qubic_list(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
});
```
#### example response
```json
{"duration":42,"success":true,"list":[{"specification":{"code":"return('hello world');","hash_period_duration":20,"result_period_duration":10,"execution_start":1537543280,"run_time_limit":10,"type":"qubic transaction","version":"ql-0.5.0"},"id":"MVEOOSTGJDMGYXE9CKYZLFFZENDDAYIG9YUWTOAFKD9UCQDORDFXX9FAHMYGCHFT9RT9PFGNEANLFB999","state":"assembly phase"}]}
```
***
### `qubic_create()`
Creates a new qubic and stores it in the persistence. life cycle will not be automized: do the assembly transaction manually.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `execution_start`  | `int (integer{1-2147483647})` | amount of seconds until (or unix timestamp for) end of assembly phase and start of execution phase
| `hash_period_duration`  | `int (integer{1-2147483647})` | amount of seconds each hash period (first part of the epoch) lasts
| `result_period_duration`  | `int (integer{1-2147483647})` | amount of seconds each result period (second part of the epoch) lasts
| `runtime_limit`  | `int (integer{1-2147483647})` | maximum amount of seconds the QLVM is allowed to run per epoch before aborting (to prevent endless loops)
| `code`  | `string (string)` | the qubic code to run
#### example call
```js
QLITE.qubic_create(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 300, 30, 30, 10, 'return(epoch^2);');
```
#### example response
```json
{"duration":42,"success":true,"qubic_id":"YVMZUIRMRY9GJBFPAXIITZPHGUFFEIFYDPOFHPGVTZPOPGWVGEBVCJINBCP9V9HAEEGICAHCQCIJSZTXU"}
```
***
### `qubic_delete()`
Removes a qubic from the persistence (private key will be deleted: cannot be undone).
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `qubic`  | `string (trytes{81})` | deletes the qubic that starts with this tryte sequence
#### example call
```js
QLITE.qubic_delete(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'OSZIVKKIISMGASEXLJTTXQCVQTYMBNWRPGOMEQKVRZUTOAYPCSVUVSGLODJKRJXPAHRIHVESAVLWSOQAM');
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `qubic_list_applications()`
Lists all incoming oracle applications for a specific qubic, response can be used for 'qubic_assembly_add'.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `qubic`  | `string (trytes{81})` | the qubic of which you want to list all applications
#### example call
```js
QLITE.qubic_list_applications(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'DUKV9BLGJYPNM9RNEJOVMRKGTUBXAUGAGUJYQKZQLYGXXLKJMFVAYRFIPIUSTNUVFPRHQFUE9K9ZPLN9Q');
```
#### example response
```json
{"duration":42,"success":true,"list":["GIM9CVAKHHXYJFUXGTH9SLYDPWIVPEGFEYDMUTRYAAPKILUCVCHVCTMNEWMSMUXJMOBZNJADA9JEZFKQW","P9HZYAITEDUFVQXILQJBBYZ9VOSQYUZRQXRBVQD9XFBPFPSHENMPLPXEXDLNFXAWNWSQAHBQRORMJJFF9"]}
```
***
### `qubic_assemble()`
Publishes the assembly transaction for a specific qubic.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `qubic`  | `string (trytes{81})` | the qubic that shall publish its assembly transaction
| `assembly`  | `array (jsonarray)` | json array of the oracle IDs to be part of the assembly
#### example call
```js
QLITE.qubic_assemble(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'RYBJWGXI9MTHGPDEIKZ9ZLSAKAP9LUHLSVKIVSILIMZHPXZG9YN9LBODXBVNUKVDBYGMKJQXVLENBGHKL', ['NVIY9QTVBHOTKCNPHNYHAEMPOVDSMNZAZDHUKGPG99PJUGTIAJZLIGLUBOWUYUZTUHCWTJQZYMLUW9RSR', 'LOQNOUWPEHKDWI9WNXRUYLKJZ9YRWXWPLVOFHWSURVRPIMPXDBGXSOULWJZZZBJUSNLROFRVBJPOWPGEJ']);
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `qubic_test()`
Runs QL code locally (instead of over the tangle) to allow the author to quickly test whether it works as intended. Limited Functionality (e.g. no qubic_fetch).
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `code`  | `string (string)` | qubic code you want to test
| `epoch_index`  (opt.) | `int (integer{0-2147483647})` | initializes the run time variable 'epoch' to simulate a running qubic
#### example call
```js
QLITE.qubic_test(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'return(epoch^2)', 3);
```
#### example response
```json
{"result":"9","duration":42,"success":true,"runtime":12}
```
***
### `qubic_consensus()`
Determines the quorum based consensus of a qubic's oracle assembly at any IAM index.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `qubic`  | `string (trytes{81})` | qubic to find consensus in
| `keyword`  | `string (trytes{0-30})` | keyword of the iam index to find consensus for
| `position`  | `int (integer{0-2147483647})` | position of the iam index to find consensus for
#### example call
```js
QLITE.qubic_consensus(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'KXSMCSMDZRZYUTOEAMVBALRTKFXVENBZUXWQZRYPTBMSMQCDAKDKRRETXENTIVUONQLZRPPUVTOBQDNQF', 'Q', 4);
```
#### example response
```json
{"result":"{'color': 'red'}","duration":42,"success":true,"index_keyword":"COLORS","quorum":3,"quorum_max":4,"index_position":2018}
```
***
### `oracle_create()`
Creates a new oracle and stores it in the persistence. Life cycle will run automically, no more actions required from here on.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `qubic`  | `string (trytes{81})` | ID of the qubic which shall be processed by this oracle.
#### example call
```js
QLITE.oracle_create(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'QPFTWIZZQUZQHLELUFFEKBVALVNZRHDUEURIQNKZUBFKAGXRTBYQJHDZQUCOZQWJKYAFWWTFKDUDKADLX');
```
#### example response
```json
{"duration":42,"oracle_id":"UOJXGICXEA9UGO9H9VMYCOVDN9DNRKLEJFWUHYQIUKVGJXNPWV9UWTOIDZSCBKBR9NARAP9SVC9GUGOCR","success":true}
```
***
### `oracle_delete()`
Removes an oracle from the persistence (private key will be deleted, cannot be undone).
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `id`  | `string (trytes{81})` | oracle ID
#### example call
```js
QLITE.oracle_delete(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'BXGS99EYAK9HMV9LHQIS99AM9QTRGPWZASCGWJPRLAZIPXECNKGOYMVJCOSKGIBQJSU9FVOHNHXWTLGCN');
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `oracle_list()`
Lists all oracles stored in the persistence
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
#### example call
```js
QLITE.oracle_list(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
});
```
#### example response
```json
{"duration":42,"success":true,"list":["LZHSSSADUZWGBWZYKYCOHKQSBIXKZGBCKRHIE9VJJJIB9YJJGPCXNTJKQZGT9NZAUMPANNBMZFSNVZLVU","WQHRNQUNFLWYORPFNHNRAKYTFWBNSNAEAMFYEJCNYRCY9UU9NDHUDHBCHHIJOSTMEJSHKC9YOONFFCTWC"]}
```
***
### `oracle_pause()`
Temporarily stops an oracle from processing its qubic after the epoch finishes. Can be undone with 'oracle_restart'.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `id`  | `string (trytes{81})` | oracle ID
#### example call
```js
QLITE.oracle_pause(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'BTO9ZMXZT9ALXBUDZWCDJYRJVFJWAMQZYB9JQGSMOYA9WMYAUEVLAWCJESACBPBRTBQ9UQNP9MZOZZWSN');
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `oracle_restart()`
Restarts an oracle that was paused with 'oracle_pause', makes it process its qubic again.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `id`  | `string (trytes{81})` | restarts the oracle that starts with this tryte sequence
#### example call
```js
QLITE.oracle_restart(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'QAMFDSMSFUENHLJJEUMFFURIQDVEKRAFXEVGLHHULRZN9FUBSBJARYBC9B9PMMHJ9PJHDMPPKWAFFQRTS');
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `iam_create()`
Creates a new IAM stream and stores it locally in the persistence.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
#### example call
```js
QLITE.iam_create(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
});
```
#### example response
```json
{"duration":42,"iam_id":"IATEVQVMFUQNEBDADBTAOEQ9XBOVJXFDJSBRUITFTZ9GJB9AOSYYBDHLMPBNKZUPJBDVFYKYJSYSUDHVX","success":true}
```
***
### `iam_delete()`
Removes an IAM stream from the persistence (private key will be deleted, cannot be undone).
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `id`  | `string (trytes{81})` | IAM stream ID
#### example call
```js
QLITE.iam_delete(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'XUYRQFPGFAMCNNRE9BMGYDWNTXLKWQBYYECSMZAMQFGHTUHSIYKVPDOUOCTUKQPMRGYF9IJSXIMKMAEL9');
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `iam_list()`
List all IAM streams stored in the persistence.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
#### example call
```js
QLITE.iam_list(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
});
```
#### example response
```json
{"duration":42,"success":true,"list":["MUCYJSLGQM9KIEFIMOMWGGQUUJMWNBNCUSLGLOGQQHKJQB9CYUDZSZBWQVAFAYSHKSGVUISGKLSCQJN9F","9JQYX9YSJP9XPN9LAGNELSVYFP9KPMJRLKWUWLHKLVNIKYHHXWLITTDDDMNPVXSSPXJJLUABEEFPLFQDN"]}
```
***
### `iam_write()`
Writes a message into the iam stream at an index position.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `id`  | `string (trytes{81})` | the IAM stream in which to write
| `index`  | `int (integer{0-2147483647})` | position of the index at which to write
| `message`  | `object (jsonobject)` | the json object to write into the stream
| `keyword`  (opt.) | `string (trytes{0-30})` | keyword of the index at which to write
#### example call
```js
QLITE.iam_write(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'CLUZILAWASDZAPQXWQHWRUBNXDFITUDFMBSBVAGB9PVLWDSYADZBPXCIOAYOEYAETUUNHNW9R9TZKU999', 17, {'day': 4}, 'ADDRESS');
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `iam_read()`
Reads the message of an IAM stream at a certain index.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `id`  | `string (trytes{81})` | IAM stream you want to read
| `index`  | `int (integer{0-2147483647})` | position of index from which to read the message
| `keyword`  (opt.) | `string (trytes{0-30})` | keyword of index from which to read the message
#### example call
```js
QLITE.iam_read(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'CLUZILAWASDZAPQXWQHWRUBNXDFITUDFMBSBVAGB9PVLWDSYADZBPXCIOAYOEYAETUUNHNW9R9TZKU999', 17, 'RESULTS');
```
#### example response
```json
{"duration":42,"read":{"habit":"antarctica","name":"penguin"},"success":true}
```
***
### `app_list()`
Lists all apps installed.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
#### example call
```js
QLITE.app_list(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
});
```
#### example response
```json
{"duration":42,"success":true,"list":[{"license":"&copy;2018 by microhash for qame.org","description":"Grow and harvest food on your own farm. The first qApp and decentralized IOTA game. The game state is entirely stored on the Tangle and validated by Qubic Lite.","id":"tanglefarm","title":"Tangle Farm","version":"v0.1","url":"http://qame.org/tanglefarm"}]}
```
***
### `app_install()`
Installs an app from an external source.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `url`  | `string (url)` | download source of the app
#### example call
```js
QLITE.app_install(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'http://qame.org/tanglefarm');
```
#### example response
```json
{"duration":42,"success":true}
```
***
### `app_uninstall()`
Uninstalls an app.
#### parameters
| name | type | description |
| - | - | - |
| `callback` | `function` | 
| `app`  | `string (alphanumeric)` | app ID (directory name in 'qlweb/qlweb-0.5.0/qapps')
#### example call
```js
QLITE.app_uninstall(function(resp, err) {
  if(err) { console.log(err); /* handle error */ }
  else { /* process response ... */ }
}, 'tanglefarm');
```
#### example response
```json
{"duration":42,"success":true}
```
***
