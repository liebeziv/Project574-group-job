**1. No UI Feedback After Saving a Routine**

**Description:**  
When editing a routine and clicking "Save", the data is successfully saved (verified via API response and cache invalidation), but **there is no UI feedback shown**. Users don't see a success message, leading to confusion about whether their changes were saved.

**Related Issue:**  
https://github.com/wger-project/react/issues/1114

**Pull Request:**  
https://github.com/wger-project/react/pull/1117

**What I Did:**  
- Created a new save button component. The new save button is disabled briefly, then shows "Save ❌" and turns red if it fails, or "Save ✅" if it succeeds, then switches back to the normal "Save" button.
- Added a `handleSave` function and replaced the save button in the routine form.

**Feedback:**  
The author felt making the button red was a bit "too much". He suggested simply disabling the button while the query is running, showing the ✅ afterwards, and then removing it after a timeout. He also suggested using the `FormQueryErrors` component to handle errors.

**What I Did Next:**  
- Imported the `FormQueryErrors` component into `RoutineForm`.
- Did not change the color of the button, kept the ❌ so that it corresponds with the ✅.
- Waiting for next feedback.

---

**2. When No Routine/Measurement Exists, the Add Button Is Not Obvious**

**Description:**  
When there are no routines/measurements added, the routine overview page does not display an "Add Routine" button—only the message "Nothing here yet... Press the action button to begin". For new users unfamiliar with the application, it may take some time to locate the add button, which could create a poor first-time user experience. A visual prompt (such as an arrow pointing to the button) should appear when no routines/measurements exist, guiding users to the action button.

**Related Issue:**  
https://github.com/wger-project/react/issues/1103

**Pull Request:**  
https://github.com/wger-project/react/pull/1105

**What I Did:**  
- Added an arrow with a pulse animation pointing to the add button based on its position.
- Created an arrow in `OverviewEmpty`, created a `fabRef` to pass it to `OverviewEmpty` and `AddRoutineFab` in `RoutineOverview`.

**Feedback:**  
The author thought the solution was to perhaps align it to the main content. The pull request was closed.

---

**3. Improve UI in Private Template Page When There Is Nothing**

**Related Issue:**  
https://github.com/wger-project/react/issues/1112  
This issue is related to issue 1103.

**Description:**  
When there are no private templates added, no button is provided. Currently, the logic is that users set a routine as a template in the routine-edit path (toggle option), not by directly adding a private template. I think the private template page could be improved when users haven’t set any private templates yet. Some possible ideas:  
1. Use the Add button to show a list of routines and let users copy one as a template.  
2. Add a question mark next to "Templates". When users click it, show some help content.

**Feedback:**  
Did not get feedback; the issue was directly closed. The author changed the way templates are handled at https://github.com/wger-project/react/pull/1113. "Templates can now be directly created from their overview, and the correct flags are set automatically."
---

**4. Unclear Label "Entries" on Calendar View**

**Related Issue:**  
https://github.com/wger-project/react/issues/1131

**Pull Request:**  
https://github.com/wger-project/react/pull/1106

**Description:**  
The label "Entries - YYYY/MM/DD" may be unclear to users. It doesn’t clearly convey what type of entries (e.g., weight, workout, meals).

**Suggested Fix:**  
1. Change label from `"Entries"` to `"Your daily records"` or `"Your log for {date}"` (need to ask).  
2. Detect if no data has been logged for the selected date and show:  
   - A general message: "No data logged yet for this day."  
   - Or detailed missing items: "You haven't logged your weight/workout/meals today."

**What I Did:**  
- Changed the label from "Entries" to "Your daily records".
- In the `CardContent`, added a `Box` to display the message if no data has been logged: "You haven't logged your weight/workout/nutrition today. Add your first record".

**Feedback:**  
Waiting for it.

---

**5. Switching the Language to Chinese Is Not Working**

**Related Issue:**  
https://github.com/wger-project/react/issues/1123

**Pull Request:**  
https://github.com/wger-project/react/pull/1124

**Description:**  
After switching the language to Chinese (either simplified or traditional), the interface does not display in Chinese. The only part shown in Chinese is the calendar view (Chinese calendar). However, the word "Calendar" itself still remains in English. "Chinese simplified" in the language menu also remains in English.

**What I Did:**  
- Created symbolic links. Added `zh -> zh_Hans` symlink for Chinese language support. 

**Feedback:**  
The author did not think adding symlinks was the right solution. He thought it would be cleaner to configure the i18n service to handle this case itself.

**Next Steps:**  
Try to fix it.

**Current Progress:**  
Found both end errors:  
- **Frontend Build Error:** Assets in public directory cannot be imported from JavaScript.  
  **Cause:** You're trying to import common from `"locales/en/translation.json"`.  
  **Suggested Fix:** Remove the direct import and let i18next load files via HTTP requests.  
- **Backend 404 Error:**  
  **Error:** `GET /static/react/locales/zh/translation.json 404`.  
  **Cause:** Files exist at `public/locales/zh_Hans/` but server expects them at `static/react/locales/zh_Hans/`.  
  **Suggested Fix:** Set up proper file copying from development to production location.

---