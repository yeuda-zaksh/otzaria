NOT READY

ענף: fix/correction-editor-keyboard @ fac89b956f546bd7589ef43cdfd36698b3ec0c16 (שני קומיטים מעל upstream/dev 7f2b6c4ec; מחבר ומבצע yeuda-zaksh; בלי ייחוס AI).
כותרת PR: העורך בתוסף התיקון: Enter מכניס ירידת שורה, חצים נעים בין שורות ופסקאות, Escape יוצא מהעריכה

סיבת NOT READY (לא טכנית; נדרשת הכרעה/פעולה אנושית):
1. תבנית ה-PR דורשת צילומי לפני/אחרי בשינוי UI. לא צולמו (אין הרצת האפליקציה בסביבה). הפרטים הנראים: פסקה מעוצבת (`אב<b>גד</b>ה`) אחרי Enter, לפני: טקסט רגיל, אחרי: עיצוב נשמר.
2. שדה `Fixes #` — אין לי מספר issue פתוח ב-upstream (הערות: Refs #5553, שנסגר). צריך מספר issue או החלטה לפתוח אחד.

מה הושלם (שני הפריטים שנשארו בהערות):
- פסקה מעוצבת שפוצלה ב-Enter נראית עכשיו מעוצבת: `_HardBreakController.buildTextSpan` מרחיב את העלים של בקר הקורא ל-\r\n (במקום ליפול לטקסט רגיל).
- ביטול/ביצוע חוזר אחרי מחיקת תו מעוצב בפסקה מפוצלת מחזיר את העיצוב: `HardBreakValue` נושא את הערך הנקי (כולל CorrectionFormat), שורד את `copyWith` של undoStackModifier (אנדרואיד), והסנכרון משתמש בערך כפי שהוצב (`stored`) ובסמן הנקי.
- שחזור (b) של המתחזק (Enter בין `בַבֹ`, מחיקת השנייה, ביטול → ניקוד שלם): נבדק בבדיקה קיימת בקומיט 1, עוברת על הענף ואדומה על dev.
- שורש ההרחבה: ניסיון ה-WIP לבדו שבר 5 בדיקות קיימות (rtl_text_field_editing/hard_break, plugin_correction_editing); תוקן (סמן נקי נלקח מהמיפוי, ולא מהערך הנקי הישן; הסנכרון קורא ל-`stored` ולא לערך העטוף).

בדיקות (קבצים שמייבאים את מה שנגעתי בו, 98 קבצים + test/design_system/ui_lint_test.dart):
- ענף: +1468 -0.
- upstream/dev: +1450 -0 (בלי קובץ ה-keyboard החדש; 3 "loading" הם קבצי עזר/fixture ללא main, וגם הקובץ החדש שלא קיים ב-dev).
- test/widgets/text + test/plugins/view על הענף: +589 ~4 -0.
- קומיט 1 על קוד dev: 11 בדיקות keyboard אדומות (כל הקבוצות החדשות), מתוכן 2 של העיצוב המפוצל.
- מוטציות: הסרת copyWith של HardBreakValue → בדיקת הביטול אדומה; ביטול ההרחבה ב-buildTextSpan → שתי בדיקות העיצוב אדומות. שוחזרו.
- flutter analyze (קבצים ששונו): ללא בעיות. dart format --set-exit-if-changed: נקי. ui_lint: test/design_system/ui_lint_test.dart עובר. generate_plugin_spec --check: "spec.json מעודכן".
- לא רצה חבילת הבדיקות המלאה.

חפיפות: #5551 (plugin_book_correction_service.dart) — git merge-tree נקי. שאר ה-PRs מ-#5540 ומעלה לא נוגעים בקבצי lib שלי (לא ניתן לקרוא רשימת PR פתוחים מהסביבה; נבדקו כל refs/pull/>=5540, כולל סגורים).
לא נקראו 5 ה-PRs הממוזגים האחרונים באזור לצורך סגנון (אין גישת GitHub ל-upstream).

החלטות פתוחות: מספר issue ל-Fixes; צילומי מסך; אישור שהקומיט השני כולל גם עדכון הציפייה ב-combined_book_screen_test ('\n' מותר בהצעה).
