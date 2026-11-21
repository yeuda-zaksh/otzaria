READY

ענף: fix/correction-editor-no-jump @ d26a30df78a11ff26674e32c70f4aa5c14ea109c (2 קומיטים על upstream/dev cee59520a; מחבר ומבצע yeuda-zaksh; בלי ייחוס AI)
כותרת PR: פסקת תיקון: מעבר מקריאה לעורך בלי קפיצה והפעלה בלחיצה אחת

בדיקות (test/plugins/view, plugin_correction_end_to_end_test, combined_book_screen_test):
- ענף: 481 עברו, 0 נכשלו, 4 דולגו
- dev: 457 עברו, 0 נכשלו, 4 דולגו
- קומיט 1 על dev: 20 מתוך 24 נכשלות (ארבע בודקות התנהגות קיימת)
- analyze, dart format, ui_lint (exit 0, בלי רשומות plugin_correction), generate_plugin_spec --check, pubspec.lock: נקיים

תיקוני QA (בתוך הקומיטים, force-with-lease): 4 בדיקות חדשות בקומיט 1 למוטציות שלא נתפסו (חלון הלחיצה הכפולה, מרחק הלחיצה הכפולה, כפתור משני, פקיעת דיכוי הגלילה); BODY תוקן (סעיפי התבנית, מספרים, חפיפות).

היסטוריית סבבים: סבב 1 - rebase אחרי מיזוג #5683, Escape עם עריכה כדגל; QA עצמאי NOT READY (מוטציה ששרדה, ענף לא מעודכן, BODY ישן). סבב 2 - תוקן; QA עצמאי שני READY והוסיף 4 בדיקות.
חיפוש PRs פתוחים עם plugin_correction (search API) לא החזיר תוצאות; לא אומת מול GitHub API מלא - המנהל מתבקש לאמת.
