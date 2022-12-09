<h1 align="center">
  Divine Binary Tags
</h1>


<p align="center">
<img src="https://divine-star-software.github.io/DigitalAssets/images/logo-small.png">
</p>

---

Divine Binary Tag is a library that allows you to use an ArrayBuffer as a tag register. 


### Example Code

```ts
import { TagManager } from "../out/TagManager.js";
import { RemoteTagManager } from "../out/RemoteTagManager.js";
const manager = new TagManager("main");
manager.registerTag({
  id: "#dbt:tnum1",
  type: "typed-number",
  numberType: "8ui",
});
manager.registerTag({
  id: "#dbt:tnum2",
  type: "typed-number",
  numberType: "32ui",
});
manager.registerTag({
  id: "#dbt:num-array",
  type: "typed-number-array",
  numberType: "8ui",
  length: 10,
});

manager.registerTag({ id: "#dbt:bool1", type: "boolean" });
manager.registerTag({ id: "#dbt:bool2", type: "boolean" });
manager.registerTag({ id: "#dbt:num1", type: "number", range: [0, 15] });
manager.registerTag({ id: "#dbt:num2", type: "number", range: [0, 15] });

const numIndexes = 3;
const data = manager.$INIT({ numberOfIndexes: numIndexes });

const buffer = new ArrayBuffer(data.bufferSize);
data.buffer = buffer;
manager.setBuffer(buffer);

manager.setTag("#dbt:bool1", 1);
manager.setTag("#dbt:num1", 15);
manager.setTag("#dbt:tnum1", 16_000);

manager.loopThroughAllIndexTags(() => {
  manager.setTag("#dbt:tnum1", (Math.random() * 255) >> 0);
  manager.setTag("#dbt:tnum2", (Math.random() * 10_000) >> 0);
});

console.log("[MAIN]");
manager.loopThroughAllIndexTags((id, value, index) => {
  console.log([index], id, "=>", value);
});

const remoteManager = new RemoteTagManager("remote");

remoteManager.$INIT(data);
console.log("[REMOTE]");
remoteManager.loopThroughAllIndexTags((id, value, index) => {
  console.log([index], id, "=>", value);
});

manager.setArrayTagValue("#dbt:num-array", 0, 2);
manager.setArrayTagValue("#dbt:num-array", 1, 10);
console.log(manager.getArrayTagValue("#dbt:num-array", 0));
console.log(manager.getArrayTagValue("#dbt:num-array", 1));
```


#### Output

```console
[MAIN]
[ 0 ] #dbt:bool1 => 1
[ 0 ] #dbt:bool2 => 0
[ 0 ] #dbt:num1 => 15
[ 0 ] #dbt:num2 => 0
[ 0 ] #dbt:tnum1 => 4551
[ 1 ] #dbt:bool1 => 0
[ 1 ] #dbt:bool2 => 0
[ 1 ] #dbt:num1 => 0
[ 1 ] #dbt:num2 => 0
[ 1 ] #dbt:tnum1 => 544
[ 2 ] #dbt:bool1 => 0
[ 2 ] #dbt:bool2 => 0
[ 2 ] #dbt:num1 => 0
[ 2 ] #dbt:num2 => 0
[ 2 ] #dbt:tnum1 => 6497
[REMOTE]
[ 0 ] #dbt:bool1 => 1
[ 0 ] #dbt:bool2 => 0
[ 0 ] #dbt:num1 => 15
[ 0 ] #dbt:num2 => 0
[ 0 ] #dbt:tnum1 => 4551
[ 1 ] #dbt:bool1 => 0
[ 1 ] #dbt:bool2 => 0
[ 1 ] #dbt:num1 => 0
[ 1 ] #dbt:num2 => 0
[ 1 ] #dbt:tnum1 => 544
[ 2 ] #dbt:bool1 => 0
[ 2 ] #dbt:bool2 => 0
[ 2 ] #dbt:num1 => 0
[ 2 ] #dbt:num2 => 0
[ 2 ] #dbt:tnum1 => 6497
2
10
```