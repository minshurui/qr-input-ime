# 盒子端同步功能补丁（短语/历史 存盒子）

让常用短语、剪贴板历史存到盒子端，换手机扫码数据不丢。
前端（assets/input.html）已改好并测试通过，**只需在 Android 工程改两个文件**。

## ⚠️ 最重要的一步

重新打包前，**用仓库里最新的 `assets/input.html` 替换你工程 assets 下的同名文件**，
否则 APK 内置的还是旧控制台，不会调用新接口。

---

## 1. InputServer.java

在 `route()` 方法里，`/api/image` 分支之后、`return newFixedLengthResponse(...NOT_FOUND...)` 之前，加：

```java
// ===== 短语/历史 盒子端存储 =====
if (uri.equals("/api/phrases") && method == NanoHTTPD.Method.GET) {
    JSONObject j = new JSONObject();
    j.put("list", new JSONArray(this.imeService.getPrefs().getString("phrases", "[]")));
    return json(j);
}
if (uri.equals("/api/phrases") && method == NanoHTTPD.Method.POST) {
    String body = getPostBody(session);
    this.imeService.getPrefs().edit().putString("phrases", body == null ? "[]" : body).apply();
    JSONObject j = new JSONObject();
    j.put("status", "ok");
    return json(j);
}
if (uri.equals("/api/history") && method == NanoHTTPD.Method.GET) {
    JSONObject j = new JSONObject();
    j.put("list", new JSONArray(this.imeService.getPrefs().getString("history", "[]")));
    return json(j);
}
if (uri.equals("/api/history") && method == NanoHTTPD.Method.POST) {
    String body = getPostBody(session);
    this.imeService.getPrefs().edit().putString("history", body == null ? "[]" : body).apply();
    JSONObject j = new JSONObject();
    j.put("status", "ok");
    return json(j);
}
```

文件头 import 补一行：

```java
import org.json.JSONArray;
```

## 2. QrInputMethodService.java

在类里任意位置加（返回本服务私有存储，供 InputServer 用）：

```java
public android.content.SharedPreferences getPrefs() {
    return getSharedPreferences("qrime", android.content.Context.MODE_PRIVATE);
}
```

---

## 接口说明（前端已按此调用）

| 方法 | 路径 | 请求/响应 |
|---|---|---|
| GET | `/api/phrases` | `{"list": ["a","b"]}` |
| POST | `/api/phrases` | body 为 JSON 数组字符串，如 `["a","b"]` |
| GET | `/api/history` | `{"list": ["a","b"]}` |
| POST | `/api/history` | body 为 JSON 数组字符串 |

前端行为：盒子端优先，接口不存在（旧版服务端）自动回退 localStorage，向后兼容。

## 验证

改完打包安装到盒子后，手机浏览器打开控制台 → 添加一个短语 → 关掉浏览器重开（或换台手机扫码）→ 短语还在，即成功。
