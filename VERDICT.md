READY

# QA — עורך התיקון: הסתרת ניקוד וטעמים (סבב 1 בענן)

- ענף: `fix/correction-editor-hide-nikud-v2` @ `ce7a4e6c5` (שני קומיטים מעל `upstream/dev` @ `f358119ef`, הענף מעודכן מול dev)
- כותרת PR: עורך התיקון: הסתרת ניקוד וטעמים כמו בתצוגה, בלי לאבד את המקור
- לא נדרש תיקון בענף; לא נדחף דבר אליו.

## מטא
- כותב ומבצע בשני הקומיטים: yeuda-zaksh (noreply). אין שום ייחוס AI בהודעות הקומיטים. בדיוק 2 קומיטים.
- אין שינוי ב-`pubspec.lock` או `pubspec.yaml`. אין קבצים שאינם קשורים (9 קבצים: 4 קבצי lib של העורך, `rtl_text_field.dart`, 4 קבצי בדיקה).
- קומיט 1 בדיקות בלבד (4 קבצי test). קומיט 2 הקוד.
- לא שונה API של התוספים, ולכן `generate_plugin_spec --check` לא נדרש.

## כשל על dev (קומיט 1 על גבי dev)
פקודה: `flutter test` על controller / projection / hide_marks / editing / combined_book_screen.
- dev + קומיט 1: **80 עברו, 26 נכשלו**, ושני קבצים לא מתקמפלים (controller_test, projection_test: ה-API `canonical`, `restoreCanonical`, `removeNikud` וקובץ projection לא קיימים ב-dev).
- בקובץ hide_marks (מתקמפל על dev) יש כשלים התנהגותיים. ארבע בדיקות ב-combined_book_screen_test נכשלות על dev (הוסרה שם הקריאה הידנית ל-`editor.onChanged`, כי השדה כבר לא משתמש ב-onChanged).
- הכשל על dev בשני הקבצים המרכזיים הוא כשל קומפילציה, לא כשל התנהגות. זה נובע ממבנה התיקון (ה-API החדש), והוכחת השמירה על ההתנהגות נעשית במוטציות למטה. יש לציין זאת בגוף ה-PR (נכלל ב-BODY.md).

## בדיקות על הענף (ספירה מדויקת)
- ארבעת קבצי העורך (controller, projection, hide_marks, editing): **153 עברו, 0 נכשלו**. עם combined_book_screen_test: **212 עברו, 0 נכשלו**.
- סט רחב (`test/plugins test/widgets/text test/text_book/view/combined_view test/tabs test/design_system test/personal_notes test/search test/shortcuts test/app_report test/widgets/search_pane_base_test.dart`):
  - הענף: +9062, ~23 דולגו, **1 נכשל**
  - `upstream/dev`: +8957, ~23 דולגו, **1 נכשל**
  - הכשל הזה זהה בשניהם: `plugin_bridge_adapter_test.dart` "כשל באמצע הכתיבה אינו פוגע בקובץ הקיים" (סביבה, הרצה כ-root). ההפרש של 105 בדיקות שעברו הוא הבדיקות החדשות.
- `flutter analyze` על 9 הקבצים: נקי. `dart format --set-exit-if-changed`: 0 שונו. `dart tool/ui_lint_cli.dart`: יציאה 0, אין הפרות בקבצי העורך או ב-`rtl_text_field`. `test/design_system/ui_lint_test.dart`: 133 עברו.

## מוטציות (כל אחת הורצה, נכשלה, ושוחזרה)
על 4 קבצי העורך (153 בדיקות):
1. הסרת דילוג הרווח/מקף שלפני פסק ב-`_align` (projection): **6 נכשלו** (בדיקת projection "שני רווחים לפני פסק" + 5 בדיקות controller של פסק).
2. ביטול ה-snapshot של ההרכבה (`final base = (state: _state, value: value)`): **10 נכשלו** (בדיקות ההרכבה, ביטול על אות נבחרת, רב-שורתי, טיוטה, היסטוריה, וביטול כשהטעמים/הניקוד מוסתרים).
3. הסרת קיצור הדרך `after.text == base.value.text` ב-`_editAfter`: **7 נכשלו** (ביטול הרכבה בכל המצבים).
כל תיקון שמור בבדיקה שנכשלת בלעדיו.

## בדיקות פרובינג חדשות שלי
Fuzz מבוסס seed (6000 איטרציות, אלפבית עם אותיות, ניקוד, טעמים, רווח כפול, מקף, פסק, שורה חדשה, כל 4 מצבי ההסתרה; מחיקה/החלפה של תווים גלויים, בחירה מפורשת ובלעדיה): (א) הטקסט המוצג שווה ל-`removeMarks(canonical)` ולערך שנערך; (ב) במחיקת תו גלוי בודד, הטקסט המלא הוא המקור פחות גוש רציף אחד שמכיל לכל היותר תו גלוי אחד; (ג) ביטול דרך `applyUndoValue` מחזיר מקור מלא זהה. **עבר, לא נמצא אובדן מקור.** לא נוסף לענף (הקובץ זמני).
סקירת הקוד של `plugin_correction_edit.dart` ושל נתיב הקלט בבקר (ביטול/הזזה/סיום הרכבה, undo, setHiding, setSource, restoreCanonical, רב-שורתי, טיוטה דרך onEdit): לא נמצא נתיב אובדן נוסף. המגבלה המוכרת (setHiding באמצע הרכבה מאפס snapshot) מקובלת.

## PRs חופפים (נבדקו ראשי PR 5650 ומעלה דרך git; מצב פתוח/סגור נבדק ב-web)
- #5735 (ypl, סגור על ידי המתחזק): החלפה, התנגשויות צפויות, לא רלוונטי.
- **#5776** (yeuda-zaksh, **פתוח**, "עורך התיקון: הדגשת תוצאות חיפוש כמו בתצוגה"): בנוי על הגישה הקודמת (5 קומיטים, כולל הקומיטים הישנים של הניקוד). `git merge-tree`: **מתנגש** ב-`plugin_correction_controller.dart`, `plugin_correction_paragraph.dart`, `plugin_correction_projection.dart` (add/add) ובשלוש בדיקות. 
- #5667 (dudua99, פתוח, נוגע ב-`text_manipulation.dart`): מתמזג נקי.
- #5740 (mmichaelush, מסמכי agent-ui + goldens של `rtl_text_field`): אין התנגשות בקבצי הענף הזה (ההתנגשויות הן בקבצים אחרים, הענף שלו ישן).
- כלי ה-API של GitHub לא נגיש למאגר upstream מהענן, לכן הסריקה היא לפי ראשי PR מ-#5650 ועד #6109 ומצב דרך דף ה-PR. מומלץ אימות סופי מקומי לפני הגשה.

## החלטות פתוחות לבעלים
1. #5776 (פתוח, חופף ומתנגש): מי נכנס ראשון? ההמלצה: להגיש את זה, ואז לבנות מחדש את #5776 על גבי הענף הזה (הוא מכיל את הגישה הישנה עם נתיבי אובדן הנתונים). בגוף ה-PR נכתב שהוא חופף.
2. תיבת "אין PR פתוח אחר על אותו נושא" בתבנית: לא מסומנת בגלל #5776.
3. גודל השינוי (כ-1850 שורות, כולל חילוץ `plugin_correction_edit.dart`, היסטוריית ביטול מותאמת ורישום UndoManager ב-iOS) גדול מתיקון מינימלי. המתחזק ביקש לטפל במקרי אובדן הנתונים ובסנכרון תצוגה-מקור יחד. הוחלט לא לשנות את הגישה.
