# vue-quick-chat
This vue component is a simple chat that can be easily imported and used in your project.

## Features
- Custom style
- Handle on type event and on message submit 
- Chat with multiple participants
- Support for async actions like message uploaded status

<img src="https://user-images.githubusercontent.com/42742621/63946619-c3eda480-ca4b-11e9-82f0-b7636eace98d.png"/>

## Instalation
```
yarn add vue-quick-chat
```
or with npm
```
npm install vue-quick-chat --save
```

## Usage
```javascript
import { Chat } from 'vue-quick-chat'
import 'vue-quick-chat/dist/vue-quick-chat.css';


export default {
  components: {
    Chat
  },
}
```
```html
<template>
  <div>
      <Chat 
       :participants="participants"
       :myself="myself"
       :messages="messages"
       :on-type="onType"
       :on-message-submit="onMessageSubmit"
       :chat-title="chatTitle"
       :placeholder="placeholder"
       :colors="colors"
       :border-style="borderStyle"
       :hide-close-button="hideCloseButton"
       :close-button-icon-size="closeButtonIconSize"
       :on-close="onClose"
       :submit-icon-size="submitIconSize"
       :load-more-messages="toLoad.length > 0 ? loadMoreMessages : null"
       :async-mode="asyncMode"/>
   </div>
</template>
```
You can also use a slot to define the header content
```html
<div>
	<Chat 
        :participants="participants"
        :myself="myself"
        :messages="messages"
        :onType="onType"
        :onMessageSubmit="onMessageSubmit"
        :chatTitle="chatTitle"
        :placeholder="placeholder"
        :colors="colors"
        :borderStyle="borderStyle"
        :hideCloseButton="hideCloseButton"
        :closeButtonIconSize="closeButtonIconSize"
        :submitIconSize="submitIconSize"
        :asyncMode="asyncMode">
        <template v-slot:header>
          <div>
            <p v-for="(participant, index) in participants" :key="index" class="custom-title">{{participant.name}}</p>
          </div>
        </template>
        </Chat>
</div>
```
Bellow we have an example of the component data structure
```javascript
import {Chat} from 'vue-quick-chat';
import 'vue-quick-chat/dist/vue-quick-chat.css';

export default {
    components: {
        Chat
    },
    data() {
        return {
            visible: true,
            participants: [
                {
                    name: 'Arnaldo',
                    id: 1
                },
                {
                    name: 'José',
                    id: 2
                }
            ],
            myself: {
                name: 'Matheus S.',
                id: 3
            },
            messages: [
                {
                    content: 'received messages',
                    myself: false,
                    participantId: 1,
                    timestamp: {year: 2019, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123}
                },
                {
                    content: 'sent messages',
                    myself: true,
                    participantId: 3,
                    timestamp: {year: 2019, month: 4, day: 5, hour: 19, minute: 10, second: 3, millisecond: 123}
                },
                {
                    content: 'other received messages',
                    myself: false,
                    participantId: 2,
                    timestamp: {year: 2019, month: 5, day: 5, hour: 10, minute: 10, second: 3, millisecond: 123}
                }
            ],
            chatTitle: 'My chat title',
            placeholder: 'send your message',
            colors: {
                header: {
                    bg: '#d30303',
                    text: '#fff'
                },
                message: {
                    myself: {
                        bg: '#fff',
                        text: '#bdb8b8'
                    },
                    others: {
                        bg: '#fb4141',
                        text: '#fff'
                    },
                    messagesDisplay: {
                        bg: '#f7f3f3'
                    }
                },
                submitIcon: '#b91010'
            },
            borderStyle: {
                topLeft: "10px",
                topRight: "10px",
                bottomLeft: "10px",
                bottomRight: "10px",
            },
            hideCloseButton: false,
            submitIconSize: "30px",
            closeButtonIconSize: "20px",
            asyncMode: false,
            toLoad: [
                {
                    content: 'Hey, John Doe! How are you today?',
                    myself: false,
                    participantId: 2,
                    timestamp: {year: 2011, month: 3, day: 5, hour: 10, minute: 10, second: 3, millisecond: 123},
                    uploaded: true,
                    viewed: true
                },
                {
                    content: "Hey, Adam! I'm feeling really fine this evening.",
                    myself: true,
                    participantId: 3,
                    timestamp: {year: 2010, month: 0, day: 5, hour: 19, minute: 10, second: 3, millisecond: 123},
                    uploaded: true,
                    viewed: true
                },
            ]
        }
    },
    methods: {
        onType: function (event) {
            //here you can set any behavior
        },
        loadMoreMessages(resolve) {
            setTimeout(() => {
                resolve(this.toLoad); //We end the loading state and add the messages
                //Make sure the loaded messages are also added to our local messages copy or they will be lost
                this.messages.unshift(...this.toLoad);
                this.toLoad = [];
            }, 1000);
        },
        onMessageSubmit: function (message) {
            /*
            * example simulating an upload callback. 
            * It's important to notice that even when your message wasn't send 
            * yet to the server you have to add the message into the array
            */
            this.messages.push(message);

            /*
            * you can update message state after the server response
            */
            // timeout simulating the request
            setTimeout(() => {
                message.uploaded = true
            }, 2000)
        },
        onClose() {
            this.visible = false;
        }
    }
}
```
## Component Props
| name | type | required |default |description |
|------|------|----------|--------|------------|
| participants | Array | true |  | An array of participants. Each [participant](#participant) should be an Object with name and id|
| myself | Object | true |  | Object of my [participant](#participant). "myself" should be an Object with name and id|
| messages | Array | true |  | An array of [messages](#message). Each message should be an Object with content, myself, participantId and timestamp|
| onType | Function | false | () => false | Event called when the user is typing |
| onMessageSubmit | Function | false | () => false | Event called when the user sends a new message |
| onClose | Function | false | () => false | Event called when the user presses the close icon |
| chatTitle | String | false | Empty String | The title on chat header |
| placeholder | String | false | 'type your message here' | The placeholder of message text input |
| colors | Object | true |  | Object with the [color's](#color) description of style properties |
| borderStyle | Object | false | { topLeft: "10px", topRight: "10px", bottomLeft: "10px", bottomRight: "10px"}  | Object with the description of border style properties |
| hideCloseButton | Boolean | false | false  | If true, the Close button will be hidden |
| submitIconSize | String | false | "15px" | The submit icon size in pixels. |
| closeButtonIconSize | String | false | "15px" | The close button icon size in pixels. |
| asyncMode | Boolean | false | false | If the value is ```true``` the component begins to watch message upload status and displays a visual feedback for each message. If the value is ```false``` the visual feedback is disabled |
| loadMoreMessages | Function | false | null | If this function is passed and you reach the top of the messages, it will be called and a loading state will be displayed until you resolve it by calling the only parameter passed to it

### participant
| name | type | description |
|---------|--------|----------------|
| id | int | The user id should be an unique value |
| name | String | The user name that will be displayed |

Example
```javascript
{
  name:  'Username',
  id: 1
},
```
### message
| name | type | description |
|---------|--------|----------------|
| content | String | The message text content |
| myself | boolean | Wether the message was sent by myself participant or by other participant |
| participantId | int | The participant's id who sent the message  |
|timestamp| Object| Object describing the year, month, day, hour, minute, second and millisecond that the message was sent |
|uploaded| Boolean| If asyncMode is ```true``` and uploaded is ```true```, a visual feedback is displayed bollow the message. If asyncMode is ```true``` and uploaded is ```false```, a visual loading feedback is displayed bollow the message. If asyncMode is ```false```, this property is ignored.|

Example
```javascript
{
  content: 'received messages', 
  myself: false,
  participantId: 1,
  timestamp: { 
    year: 2019, 
    month: 3, 
    day: 5, 
    hour: 20, 
    minute: 10, 
    second: 3, 
    millisecond: 123 
  },
  uploaded: true,
}
```
### color
| name | type | description |
|---------|--------|----------------|
| header | Object | Object containing the header background and text color |
| message | Object | Object containing the message background and text color. The Object should contains the style for 'myself' and 'others' |
| messagesDisplay | Object | Object containing the  background color of mesages container. |
| submitIcon | String | The color applied to the send message button icon |

Example
```javascript
{
  header:{
    bg: '#d30303',
    text: '#fff'
  },
  message:{
    myself: {
      bg: '#fff',
      text: '#bdb8b8'
    },
    others: {
      bg: '#fb4141',
      text: '#fff'
    }
  },
  messagesDisplay: {
    bg: '#f7f3f3'
  },
  submitIcon: '#b91010'
}
```
## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
