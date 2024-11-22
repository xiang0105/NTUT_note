# **第 3 章：增強型 E-R 模型**

## **1. 基本概念**

- **增強型 E-R 模型（EER）**：擴展了傳統 E-R 模型，包含更多建模結構。  
- **Supertype**：通用實體類型，與一個或多個子類型相關聯。  
- **Subtype**：實體子分組，具有不同於其他子分組的屬性。  
- **屬性繼承**：子類型繼承父類型的所有屬性與關係。

---

## **2. Generalization 與 Specialization**

1. **Generalization**：將多個特定實體類型抽象為一個通用實體類型（自下而上）。  
   - 例：將 CAR、TRUCK 和 MOTORCYCLE 抽象為 VEHICLE。
2. **Specialization**：從一個通用實體類型定義多個子類型（自上而下）。  
   - 例：將 PART 分為 MANUFACTURED PART 與 PURCHASED PART。

---

## **3. Supertype/Subtype 關係的約束條件**

1. **完整性約束（Completeness Constraints）**：
   - **Total Specialization**：每個父類型實例必須屬於至少一個子類型（雙線）。  
   - **Partial Specialization**：父類型實例可能不屬於任何子類型（單線）。  

2. **不相交約束（Disjointness Constraints）**：
   - **Disjoint Rule**：父類型實例只能屬於一個子類型。  
   - **Overlap Rule**：父類型實例可以同時屬於多個子類型。

3. **Subtype Discriminator**：決定實例所屬子類型的屬性。
   - **Disjoint**：單一屬性，值指示子類型。  
   - **Overlapping**：多屬性，使用布林值表示所屬子類型。

---

## **4. 實體群組（Entity Clusters）**

- 當 EER 圖過於複雜時，可將相關實體與關係分組為單一抽象實體。  
- 目標：提高 EER 圖的可讀性和管理性。

---

## **5. 預製數據模型（Packaged Data Models）**

1. **定義**：預定義的數據模型，可作為數據建模項目的起點。  
2. **類型**：通用模型或行業專用模型。  
3. **優勢**：
   - 節省時間與成本。
   - 減少錯誤，支持靈活擴展。
   - 加強跨系統整合。

---

## **結論**

本章介紹了 EER 模型的增強功能，強調了 Supertype/Subtype 關係的建模技術和約束條件，並提供了處理複雜數據結構的有效方法。通過使用實體群組和預製數據模型，可以進一步提高模型的可讀性和靈活性。
