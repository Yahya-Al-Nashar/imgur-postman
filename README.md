# imgur-postman

[![Postman](https://img.shields.io/badge/Postman-FF6C37?logo=postman&logoColor=white)](https://www.postman.com)
[![Imgur API](https://img.shields.io/badge/Imgur%20API-v3-1BB76E)](https://api.imgur.com)
[![Auth](https://img.shields.io/badge/Auth-OAuth2-blue)](https://api.imgur.com/oauth2)

End-to-end **API test collections** for the **Imgur REST API** covering valid and invalid scenarios across Images, Albums, Comments, and Gallery sharing.

API Documentation
Link: [Imgur API Docs](https://apidocs.imgur.com)

---

## 🚀 Key Features

- **End-to-End Test Coverage** for Imgur:
  - Images, Albums, Comments, Gallery Sharing, Rate Limits.
- **Valid & Invalid Scenario Separation** across 3 collections.
- **Dynamic Variable Chaining** — hashes and IDs saved automatically between requests.
- **Consistent Assertions** on every request:
  - Status code, response body, data field, success flag.
- **Environment-Based Configuration** for easy credential management.

---

## 🛠️ Technologies Used

| Component      | Technology          |
|----------------|---------------------|
| Tool           | Postman             |
| API Under Test | Imgur REST API v3   |
| Auth           | OAuth2 Bearer Token |
| Data Format    | JSON / form-data    |
| Runner         | Postman Collection Runner / Newman |

---

## 📂 Project Structure

```
imgur-postman/
├── ValidScenario.postman_collection.json       # Happy-path E2E flow
├── InvalidScenario_1.postman_collection.json   # Negative test set 1
├── InvalidScenario_2.postman_collection.json   # Negative test set 2
└── imgurEnv.postman_environment.json           # Environment variables
```

---

## ✅ Valid Scenario — Test Flow

| # | Request | Method | Description |
|---|---------|--------|-------------|
| 1 | Limit | GET | Check rate limit headers |
| 2 | Upload Image 1 | POST | Upload image, save `imageHash` + `deleteHash` |
| 3 | Get Image 1 | GET | Retrieve image by `imageHash` |
| 4 | Share Image 1 | POST | Share image to gallery with title, topic, tags |
| 5 | Upload Image 2 | POST | Upload second image, save `imageHash2` + `deleteHash2` |
| 6 | Get Image 2 | GET | Retrieve second image |
| 7 | Share Image 2 | POST | Share second image to gallery |
| 8 | Create Album | POST | Create album (privacy: public) |
| 9 | Get Album | GET | Retrieve album by `albumHash` |
| 10 | Update Album | PUT | Add both images, set title, description, cover |
| 11 | Upload Comment | POST | Post comment on Image 1, save `commentId` |
| 12 | Get Comment | GET | Retrieve comment by `commentId` |
| 13 | Delete Comment | DELETE | Delete the comment |
| 14 | Delete Album | DELETE | Delete the album |
| 15 | Delete Images | DELETE | Delete both uploaded images |

---

## 🔐 Authentication

| Auth Type | Used For |
|-----------|----------|
| `Bearer {{accessToken}}` | Write operations (upload, share, create, delete) |
| `Client-ID {{clientId}}` | Public read operations (get album, get comment) |

---

## ⚙️ Environment Variables

| Variable | Description |
|----------|-------------|
| `BaseURL` | `https://api.imgur.com/3` |
| `accessToken` | OAuth2 Bearer token |
| `clientId` | Imgur app Client ID |
| `ImageEndPoint` | `/image/` |
| `AlbumEndPoint` | `/album/` |
| `ShareEndPoint` | `/gallery/image/` |
| `CommentEndPoint` | `/comment/` |
| `LimitEndPoint` | `/credits` |
| `imageHash` | Set dynamically after Upload Image 1 |
| `imageHash2` | Set dynamically after Upload Image 2 |
| `deleteHash` | Set dynamically after Upload Image 1 |
| `deleteHash2` | Set dynamically after Upload Image 2 |
| `albumHash` | Set dynamically after Create Album |
| `albumDeleteHash` | Set dynamically after Create Album |
| `commentId` | Set dynamically after Upload Comment |

---

## 🔧 Setup & Execution

```bash
# 1. Import all collection files into Postman
# 2. Import imgurEnv.postman_environment.json and set as active environment
# 3. Fill in accessToken and clientId with your Imgur app credentials
# 4. Update image src paths in Upload Image requests to local files on your machine

# Run via Newman (CLI)
npm install -g newman
newman run ValidScenario.postman_collection.json -e imgurEnv.postman_environment.json
newman run InvalidScenario_1.postman_collection.json -e imgurEnv.postman_environment.json
newman run InvalidScenario_2.postman_collection.json -e imgurEnv.postman_environment.json
```
