presentation linkk https://slidesgo.com/editor/share/9d9a116d-34da-496d-9a73-a19dc174ae5e?embed=0&expires=1749048352&signature=19f2ab3807aabb99c9397e502fece3780450c3955ebdc7158fbd7dce726fbb10

1. ![image](https://github.com/user-attachments/assets/a76ec336-c4d2-47a5-84c2-a2854e27f3fa)

2. ![image](https://github.com/user-attachments/assets/f9f3419f-931b-4ba6-bb7b-15980e0c8b2f)

3. how to create a node
4. you can create a folder with name of the node
5. and add the html part in a file and its bpmnxml convertion into seperate file also keep a index file and inside the index file code should be like this

```   
import { NODE_TYPES } from "../../utils/constants";
import Add from "./Add";
import convertJsonToBpmnXML from "./bpmHelper";
import { StyleServices } from "@formsflow/service";

const secondaryColor = StyleServices.getCSSVariable('--no-code-secondary');

const node =  {
    details:{
        type: NODE_TYPES.ADD,  color: secondaryColor ,
        title: "Add Node", 
        description: "Adding new node",
        attributes: {},
        convertToXML: convertJsonToBpmnXML
    },
    component: Add
  };

export default node;
```
