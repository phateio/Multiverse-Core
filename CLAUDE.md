# CLAUDE.md

本檔提供 Claude Code 在此 repo 中操作的專屬指引。通用規則見 `~/.claude/CLAUDE.md` 與 workspace 根目錄的 `CLAUDE.md`，本檔僅記錄此專案的差異。

**Editing this file:** Consider the whole document before changing it — right section, right wording, every sentence in its most essential form. **Hard cap: 200 lines** — trim or consolidate before adding.

## Repository 性質

- **Fork**：`phateio/Multiverse-Core`（origin）fork 自 `Multiverse/Multiverse-Core`（upstream）。
- **預設 branch**：`main-fork`（origin 的 default）。upstream 對應 `main`。
- **PR 流向**：
  - 修改僅自用 → PR 進 `phateio/Multiverse-Core:main-fork`。
  - 預期回饋上游 → 從 `upstream/main` 開 branch，PR 進 `Multiverse/Multiverse-Core:main`。
- **不直接 push `main-fork`**，一律 feature branch + PR。
- 同步 upstream 用 merge（保留 fork 自有 commit）；rebase 前先確認沒有已 push 的共用 commit。

## Tech Stack

- Java 17（`fix/java-17-issue` 已將相容性回退到 17，不要再用 21+ 語法）。
- Gradle wrapper（`./gradlew`）。
- 套件名：`org.mvplugins.multiverse.core`。
- Paper/Spigot API 1.21.8-R0.1-SNAPSHOT，`api-version: 1.13`。
- 測試：MockBukkit 4.84.0（server API 1.21）。
- 框架：ACF（command）、HK2（DI）、Vavr、CommentedConfiguration。

## 常用指令

```bash
./gradlew build              # 編譯 + 測試 + shadowJar
./gradlew test               # 只跑測試
./gradlew checkstyleMain     # checkstyle 主程式
./gradlew checkstyleTest     # checkstyle 測試
./gradlew shadowJar          # 產生 relocated fat jar
```

CI 跑 checkstyle + test（`.github/workflows/pr.*.yml`），送 PR 前本地至少跑 `./gradlew build`。

## Code Style

- **Google Java Style Guide**（見 `CONTRIBUTING.md`）。
- Checkstyle 規則：`config/mv_checks.xml`。
- 註解語言：英文（沿用既有風格）。
- Self-documenting code 優先，避免冗註解。

## Commit / PR

- Conventional Commits（`type(scope): description`）。
- PR 標題短、聚焦單一關注點。
- 若 commit 已被 upstream merge，不要在原 branch 再 push 新 commit；開新 branch。

## 編輯注意事項

- 修改 `build.gradle` 的 `shadowJar` relocate 區塊時，確認對應的 import / 反射路徑同步調整。
- `plugin.yml` 走 `${version}` token 替換，不要寫死版本。
- `prepareSource` task 會做 `@bitly-access-token@` token 替換，編輯 source 時保留 token 字面值。
- 修改 `src/main/resources/locale/` 後，可用 `config/convert-locale-enum.py` 轉換（如有需要）。

## 不要做

- 不要把 Java 語法升到 17 以上（含 records pattern matching 之外的新特性、sealed 等請先確認 target）。
- 不要直接 push `main-fork` 或 upstream branch。
- 不要刪 `CONTRIBUTING.md` 的 Claude 指示尾註。
