# Nuclear Revit Type Purger (DeleteAndPurgeTypeByName.dyn)

A powerful, highly resilient Dynamo script designed for Autodesk Revit that hunts down specific element types by their name, forcefully removes all instances of that type placed in the model, and then permanently purges the underlying type definitions from your project database.

## 🚀 Features

- **Nuclear Search Algorithm:** Bypasses standard `WhereElementIsElementType` limitations by scanning the entire Revit project database (both Types and Instances). This guarantees that even non-standard API classes (like Graphic Styles, Line Patterns, and tricky System Families) are successfully identified.
- **Cascade-Delete Safe:** Automatically handles Revit's intrinsic cascade-delete transactions. (When Revit deletes an `ElementType`, it silently annihilates its instances in the background. The script intercepts `InvalidObjectException` errors to avoid locking up).
- **Fuzzy/Partial Name Hints:** If you make a typo, the script won't just fail silently! It outputs a highly detailed `Log and Hints` list containing all elements that partially match your query, exposing their true API internal name and object class.
- **Case-Insensitive & Whitespace Proof:** Completely ignores accidental trailing spaces or mismatched capital letters.

## ⚠️ Warning: Destructive Action
**This script makes permanent modifications to your Revit database.**
Because it performs hard deletions wrapped in a `TransactionManager` commit, anything deleted will immediately disappear from your model. Always ensure your string node isn't too vague (e.g., typing "Wall" might delete everything with "Wall" in the name if you aren't careful, though it targets exact matches preferentially). 

## 📖 How to Use

1. **Open your Revit Project.**
2. Launch **Dynamo** and open `DeleteAndPurgeTypeByName.dyn`.
3. Locate the `String` input node on the left side of the canvas.
4. Type the exact name of the Element Type you wish to destroy (e.g., `Linear - 3/32" Trebuchet MS`).
5. Click **Run** at the bottom left of Dynamo.
6. **Review the Output log** from the Python script to see exactly what was destroyed.

### Debugging Hints
If the script reports `0 destroyed`:
- Check the **Log and Hints** output list! 
- What you see in the Revit UI is often a concatenation of `[Family Name] - [Type Name]`. The underlying API `Name` property may just be the Type Name alone. The hints list will tell you exactly what the API thinks the object is called.

## 🛠️ Technical Details
- **Engine:** Built and tested explicitly against CPython3 in Dynamo.
- **API Security Bypass:** Avoids `FilteredElementCollector` unfiltered blockages by concatenating `WhereElementIsElementType()` and `WhereElementIsNotElementType()` arrays.

I didnt write this, AI did
