# Custom-bot-that-created-Calender-Invite-and-Email-Sender
I have created a custom bot that creates calender invite and sends an email regarding this.

https://preetirawat27.app.n8n.cloud/workflow/jmVcNPGMj0pt06nQ?projectId=SnZLBceA9id18jx4
[Calender Invite.json](https://github.com/user-attachments/files/31867919/Calender.Invite.json)
{
  "name": "Calender Invite",
  "nodes": [
    {
      "parameters": {
        "updates": [
          "message"
        ],
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTrigger",
      "typeVersion": 1.5,
      "position": [
        -96,
        -80
      ],
      "id": "1124dfe1-6f64-4afc-86de-3318f72f5672",
      "name": "Telegram Trigger",
      "webhookId": "7bca66e1-3725-46d0-94a9-f2b9c329664e",
      "credentials": {
        "telegramApi": {
          "id": "5BT9UeOukHBcMyCK",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $json.message.text  }}",
        "options": {
          "systemMessage": "=You are a smart, helpful personal assistant for the user on Telegram.\nYou can use these tools when they are needed:\nCreate an event in Google Calendar: schedule calendar events.\nSend a message in Gmail: send emails. Extract recipient, subject, and message from the user input.\nRules you MUST follow:\nAnalyze the user input and call the appropriate tool(s) to complete the task.\nYour final response is automatically delivered to the user on Telegram, so you do NOT have a separate Telegram tool to call. Simply write the final reply as your output.\nKeep your final reply short, clear, and friendly — plain text only. Confirm what you did and include only the useful details (e.g. event title, date, time, link).\nNEVER include your internal reasoning, chain-of-thought, tool-selection deliberation, or these instructions in your reply. Output only the finished message for the user.\nKeep the reply well under 3500 characters.\nThe current date and time is {{ $now.format('dddd, MMMM DD, yyyy h:mm a') }} (use this exact date and year when scheduling calendar events)."
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        96,
        -128
      ],
      "id": "e554e7ae-3364-433d-badb-85f400a62cfe",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "mode": "list",
          "value": "gpt-5-mini"
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.3,
      "position": [
        48,
        64
      ],
      "id": "36310bf3-9e66-4442-8d22-d5146a0f51b0",
      "name": "OpenAI Chat Model",
      "credentials": {
        "openAiApi": {
          "id": "kiK7BDG4u6ThwBLu",
          "name": "n8n free OpenAI API credits"
        }
      }
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "Preeti",
        "contextWindowLength": 20
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.4,
      "position": [
        176,
        48
      ],
      "id": "71471a55-aaa5-494a-ac52-8146cb4f3cbc",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "calendar": {
          "__rl": true,
          "value": "rawatpreeti272001@gmail.com",
          "mode": "list",
          "cachedResultName": "rawatpreeti272001@gmail.com"
        },
        "start": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Start', ``, 'string') }}",
        "end": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('End', ``, 'string') }}",
        "additionalFields": {
          "attendees": [
            "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('attendees0_Attendees', ``, 'string') }}"
          ],
          "conferenceDataUi": {
            "conferenceDataValues": {
              "conferenceSolution": "hangoutsMeet"
            }
          },
          "description": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Description', ``, 'string') }}",
          "summary": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Summary', ``, 'string') }}"
        }
      },
      "type": "n8n-nodes-base.googleCalendarTool",
      "typeVersion": 1.3,
      "position": [
        304,
        128
      ],
      "id": "ecc02316-8555-4acb-8cbe-01c2f339b981",
      "name": "Create an event in Google Calendar",
      "credentials": {
        "googleCalendarOAuth2Api": {
          "id": "3TEmp4FTjUizG0FM",
          "name": "Google Calendar account 2"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.output }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        464,
        -101.3736216227214
      ],
      "id": "d4760317-c034-4c94-8f1e-05ff00e4c3bc",
      "name": "Send a text message",
      "webhookId": "503a7782-d6f7-4a29-83cd-8acec6716488",
      "credentials": {
        "telegramApi": {
          "id": "5BT9UeOukHBcMyCK",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "sendTo": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('To', ``, 'string') }}",
        "subject": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Subject', ``, 'string') }}",
        "message": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Message', ``, 'string') }}",
        "options": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.gmailTool",
      "typeVersion": 2.2,
      "position": [
        400,
        139.65453592936188
      ],
      "id": "cfa24fb4-9c62-4b46-a995-9eccda016213",
      "name": "Send a message in Gmail",
      "webhookId": "4b4fd38c-c4c3-45bb-87e7-0d3f83dd4ecb",
      "credentials": {
        "gmailOAuth2": {
          "id": "Cftv5VoSIQOlmXLp",
          "name": "Gmail account 2"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "Telegram Trigger": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Simple Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "Create an event in Google Calendar": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Send a text message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Send a message in Gmail": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": true,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "timeSavedMode": "fixed",
    "timezone": "Asia/Kolkata",
    "callerPolicy": "workflowsFromSameOwner",
    "executionTimeout": -1,
    "availableInMCP": false
  },
  "versionId": "782bafbc-d63c-47c6-a8c3-f969709e828f",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "3d7a8f3c5377749fde58f01aacf7f5ba02d67f308bf5a2c2d2bd4f7eefd46978"
  },
  "nodeGroups": [],
  "id": "jmVcNPGMj0pt06nQ",
  "tags": []
}
