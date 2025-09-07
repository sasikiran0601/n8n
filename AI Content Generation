{
  "name": "My workflow 2",
  "nodes": [
    {
      "parameters": {
        "formTitle": "AI content generator",
        "formDescription": "Give a topic, and the AI takes care of the rest!",
        "formFields": {
          "values": [
            {
              "fieldLabel": "Content type",
              "fieldType": "textarea"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.formTrigger",
      "typeVersion": 2.2,
      "position": [
        -1472,
        16
      ],
      "id": "4c48a9b7-eed9-4ea3-abd7-8f46f182ca57",
      "name": "On form submission",
      "webhookId": "63867d9d-7cb5-4ba5-895a-1301a8cabc0c"
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "mode": "list",
          "value": "gpt-4.1-mini"
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.2,
      "position": [
        -1040,
        256
      ],
      "id": "f3911d49-ee51-436f-a55e-07dbdbd7f7b4",
      "name": "OpenAI Chat Model",
      "credentials": {
        "openAiApi": {
          "id": "NHSzbp6xjyYkXMVt",
          "name": "OpenAi account"
        }
      }
    },
    {
      "parameters": {
        "text": "={{ $json['Content type'] }}\nBased on this Topic Generate Perfect And Social Media Platform Based Captions. Don't use (+) this star symbol in the output.",
        "attributes": {
          "attributes": [
            {
              "name": "Facebook captions",
              "description": "Captions for Facebook post with proper hashtags,"
            },
            {
              "name": "Pinterest Captions",
              "description": "Captions for Pinterest post with proper hashtags"
            }
          ]
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.informationExtractor",
      "typeVersion": 1.2,
      "position": [
        -1216,
        16
      ],
      "id": "03f3aa1d-910c-446f-9bbf-5575ec911a75",
      "name": "Text Content Generation"
    },
    {
      "parameters": {
        "text": "={{ $('On form submission').item.json['Content type'] }}\n",
        "attributes": {
          "attributes": [
            {
              "name": "Image prompt",
              "description": "Prompt of the Image. Perfect for google/gemini-2.5-flash-image-preview: free model.\nEscape all double quotes inside a JSON string with a backslash (like this: \\\")"
            }
          ]
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.informationExtractor",
      "typeVersion": 1.2,
      "position": [
        -864,
        16
      ],
      "id": "b8e8560a-f58e-4356-b17d-8ef366bc695c",
      "name": "Image Content Generation"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://openrouter.ai/api/v1/chat/completions",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "openRouterApi",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "Authorization",
              "value": "Bearer $OPENROUTER_API_KEY"
            }
          ]
        },
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={\n    \"model\": \"google/gemini-2.5-flash-image-preview: free\",\n    \"messages\": [\n        {\n            \"role\": \"user\",\n            \"content\": [\n                {\n                    \"type\": \"text\",\n                    \"text\": \"{{ $json.output['Image prompt'] }}\"\n                }\n            ]\n        }\n    ]\n}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -512,
        16
      ],
      "id": "7b09d668-9c44-4433-a591-9207d0d8359b",
      "name": "HTTP Request",
      "credentials": {
        "openRouterApi": {
          "id": "y7wGsOZcVpotdDp9",
          "name": "OpenRouter account"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "e20d617a-b065-4ce4-8862-c164c99af4c7",
              "leftValue": "={{ $json.choices[0].message.images[0].image_url.url }}",
              "rightValue": "=",
              "operator": {
                "type": "string",
                "operation": "exists",
                "singleValue": true
              }
            },
            {
              "id": "075aec71-b251-4928-b767-58e5667d3018",
              "leftValue": "={{ $json.choices[0].message.images[0].image_url.url }}",
              "rightValue": "",
              "operator": {
                "type": "string",
                "operation": "notEmpty",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        -272,
        16
      ],
      "id": "9dc2067d-7e94-4d72-bb22-457a61ec1ee1",
      "name": "If"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 3,
      "position": [
        -400,
        304
      ],
      "id": "793cdbf7-bb43-4658-8f10-c003df5b9ac7",
      "name": "Loop Over Items"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "edae2d32-2abe-4b19-9af8-b41362ac0d24",
              "name": "data",
              "value": "={{ $json.choices[0].message.images[0].image_url.url }}",
              "type": "string"
            },
            {
              "id": "2e0ee5d3-3b57-4872-aa6b-2d55a425e0bd",
              "name": "base",
              "value": "={{ $json.choices[0].message.images[0].image_url.url.split(',')[1] }}",
              "type": "string"
            },
            {
              "id": "137fe338-b386-4373-8c15-abb30529233f",
              "name": "mime",
              "value": "={{ $json.choices[0].message.images[0].image_url.url.split(';')[0].split(':')[1]  }}",
              "type": "string"
            },
            {
              "id": "62d3747a-568e-4135-8853-b0b2312ca85b",
              "name": "image",
              "value": ".png",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        0,
        0
      ],
      "id": "dc761ca9-01e3-472c-b031-eee6b9f12c86",
      "name": "Edit Fields"
    },
    {
      "parameters": {
        "operation": "toBinary",
        "sourceProperty": "base",
        "options": {}
      },
      "type": "n8n-nodes-base.convertToFile",
      "typeVersion": 1.1,
      "position": [
        288,
        0
      ],
      "id": "f49fb024-8a1a-4f9f-8aa8-926d601b8bfa",
      "name": "Convert to File"
    },
    {
      "parameters": {
        "operation": "sendAndWait",
        "sendTo": "bandisasikiran@gmail.com",
        "subject": "=Approval for {{ $('On form submission').item.json['Content type'] }} Post",
        "message": "=<!DOCTYPE html>\n<html lang=\"en\">\n<head>\n  <meta charset=\"UTF-8\">\n  <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n  <title>Facebook Content</title>\n  <style>\n    /* Google Fonts */\n    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');\n\n    body {\n      font-family: 'Inter', sans-serif;\n      background: linear-gradient(135deg, #e0f7fa, #e3f2fd);\n      margin: 0;\n      padding: 20px;\n      display: flex;\n      justify-content: center;\n      align-items: center;\n      min-height: 100vh;\n    }\n\n    .container {\n      background: #fff;\n      padding: 40px;\n      border-radius: 20px;\n      box-shadow: 0px 10px 25px rgba(0, 0, 0, 0.1);\n      max-width: 800px;\n      width: 100%;\n    }\n\n    h1 {\n      text-align: center;\n      font-weight: 700;\n      margin-bottom: 20px;\n      color: #0d47a1;\n    }\n\n    h1::after {\n      content: \"\";\n      width: 80px;\n      height: 4px;\n      background: #0d47a1;\n      display: block;\n      margin: 10px auto 0;\n      border-radius: 2px;\n    }\n\n    .section {\n      margin-bottom: 25px;\n      background: #f5f9ff;\n      border-left: 6px solid #0d47a1;\n      padding: 18px 20px;\n      border-radius: 12px;\n      transition: transform 0.2s ease, box-shadow 0.2s ease;\n    }\n\n    .section:hover {\n      transform: translateY(-3px);\n      box-shadow: 0 6px 15px rgba(0, 0, 0, 0.08);\n    }\n\n    .section-title {\n      font-weight: 600;\n      font-size: 1.05rem;\n      color: #0d47a1;\n      margin-bottom: 10px;\n    }\n\n    .image-container .section-title {\n      text-align: center;\n      margin-bottom: 15px;\n    }\n\n    .image-container img {\n      max-width: 100%;\n      border-radius: 15px;\n      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.12);\n      transition: transform 0.3s ease;\n    }\n\n    .image-container img:hover {\n      transform: scale(1.03);\n    }\n\n    /* Animation */\n    @keyframes fadeIn {\n      from {\n        opacity: 0;\n        transform: translateY(15px);\n      }\n      to {\n        opacity: 1;\n        transform: translateY(0);\n      }\n    }\n\n    .section, .image-container {\n      animation: fadeIn 0.8s ease forwards;\n    }\n  </style>\n</head>\n<body>\n  <div class=\"container\">\n    <h1>✨ Facebook Content ✨</h1>\n\n    <div class=\"section\">\n      <div class=\"section-title\">Facebook Caption</div>\n      <p>{{ $('Text Content Generation').item.json.output['Facebook captions'] }}</p>\n    </div>\n\n    <div class=\"image-container\">\n      <div class=\"section-title\">Generated Image</div>\n      <img src='{{ $('If').item.json.choices[0].message.images[0].image_url.url }}' alt=\"Generated Image\">\n    </div>\n  </div>\n</body>\n</html>\n",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.1,
      "position": [
        576,
        80
      ],
      "id": "5c2f2ac2-b787-4e98-8b29-a810aee711d0",
      "name": "Send message and wait for response",
      "webhookId": "bb978c8a-9cec-419d-8683-328b9757254d",
      "credentials": {
        "gmailOAuth2": {
          "id": "QAhTiPahXaYaZbRC",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.data.approved }}",
                    "rightValue": "",
                    "operator": {
                      "type": "boolean",
                      "operation": "true",
                      "singleValue": true
                    },
                    "id": "2d60e078-93ac-4f3d-8818-49a86f73cef4"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "Approved"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "88bcea95-f42e-4fef-b6b1-a0ff9110c5d2",
                    "leftValue": "={{ $json.data.approved }}",
                    "rightValue": "",
                    "operator": {
                      "type": "boolean",
                      "operation": "false",
                      "singleValue": true
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "Regenerate"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "0a72a3bc-21fd-40ca-b5e4-c82b8cc5888e",
                    "leftValue": "={{ $json.data.approved }}",
                    "rightValue": "",
                    "operator": {
                      "type": "boolean",
                      "operation": "empty",
                      "singleValue": true
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "No Action"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.2,
      "position": [
        784,
        80
      ],
      "id": "467246cd-aa13-45b2-bb77-a4f3d3ada5cc",
      "name": "Switch"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        912,
        320
      ],
      "id": "41575f5e-030a-4872-b47d-3486cd12ccb6",
      "name": "No Operation, do nothing"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "edae2d32-2abe-4b19-9af8-b41362ac0d24",
              "name": "data",
              "value": "={{ $('If').item.json.choices[0].message.images[0].image_url.url }}",
              "type": "string"
            },
            {
              "id": "2e0ee5d3-3b57-4872-aa6b-2d55a425e0bd",
              "name": "base",
              "value": "={{ $('If').item.json.choices[0].message.images[0].image_url.url.split(',')[1] }}",
              "type": "string"
            },
            {
              "id": "137fe338-b386-4373-8c15-abb30529233f",
              "name": "mime",
              "value": "={{ $('If').item.json.choices[0].message.images[0].image_url.url.split(';')[0].split(':')[1] }}",
              "type": "string"
            },
            {
              "id": "62d3747a-568e-4135-8853-b0b2312ca85b",
              "name": "image",
              "value": ".png",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        1216,
        144
      ],
      "id": "bd1351f8-4451-4ec9-9897-e95affea4535",
      "name": "Edit Fields1"
    },
    {
      "parameters": {
        "operation": "toBinary",
        "sourceProperty": "base",
        "options": {}
      },
      "type": "n8n-nodes-base.convertToFile",
      "typeVersion": 1.1,
      "position": [
        1504,
        144
      ],
      "id": "427c1914-a454-46e5-aae7-3fccf9d003b8",
      "name": "Convert to File1"
    },
    {
      "parameters": {
        "user": "Sasikiran",
        "platform": [
          "facebook"
        ],
        "title": "={{ $('Text Content Generation').item.json.output['Facebook captions'] }}",
        "photos": "data",
        "caption": "={{ $('Text Content Generation').item.json.output['Facebook captions'] }}",
        "facebookPageId": "793272297202302"
      },
      "type": "n8n-nodes-upload-post.uploadPost",
      "typeVersion": 1,
      "position": [
        1728,
        144
      ],
      "id": "3bdfbd3f-ed87-44da-adb9-d730632883f3",
      "name": "Upload Post",
      "credentials": {
        "uploadPostApi": {
          "id": "BPOa5WpzBBgIT6FE",
          "name": "Upload Post account"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "On form submission": {
      "main": [
        [
          {
            "node": "Text Content Generation",
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
            "node": "Text Content Generation",
            "type": "ai_languageModel",
            "index": 0
          },
          {
            "node": "Image Content Generation",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Text Content Generation": {
      "main": [
        [
          {
            "node": "Image Content Generation",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Image Content Generation": {
      "main": [
        [
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request": {
      "main": [
        [
          {
            "node": "If",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If": {
      "main": [
        [
          {
            "node": "Edit Fields",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Loop Over Items",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Loop Over Items": {
      "main": [
        [],
        [
          {
            "node": "Image Content Generation",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields": {
      "main": [
        [
          {
            "node": "Convert to File",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Convert to File": {
      "main": [
        [
          {
            "node": "Send message and wait for response",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Send message and wait for response": {
      "main": [
        [
          {
            "node": "Switch",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Switch": {
      "main": [
        [
          {
            "node": "Edit Fields1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Text Content Generation",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "No Operation, do nothing",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields1": {
      "main": [
        [
          {
            "node": "Convert to File1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Convert to File1": {
      "main": [
        [
          {
            "node": "Upload Post",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "eee9c7eb-db21-4a04-8e04-82a9bf630728",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "558d88703fb65b2d0e44613bc35916258b0f0bf983c5d4730c00c424b77ca36a"
  },
  "id": "L958C6TOZmfkMrhP",
  "tags": []
}
