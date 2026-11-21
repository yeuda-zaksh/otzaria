READY

ענף: fix/correction-editor-hide-nikud ב-yeuda-zaksh/otzaria, SHA e3faf9d1773f2e5422066c860b6a7e41f2c9e7b9 (שני קומיטים מעל upstream/dev cee59520a).
כותרת PR: עורך התיקון: הסתרת ניקוד וטעמים כמו בתצוגה, בלי לאבד את המקור

## היסטוריית סבבים
- סבב 1: מימוש לפי ההערות. QA עצמאי החזיר READY אחרי שתיקן בעצמו: קיפל את שינויי הקובץ של קומיט 1 שנכנסו בטעות לקומיט 2 (קומיט 1 לא עבר format), ביצע rebase על dev הנוכחי, והוסיף בדיקה לקומיט 1 למוטציה ששרדה (גיזום תחילית/סיומת בתוך טווח הרכבה).

## בדיקות (אותו היקף בשני הצדדים: test/plugins/view, combined_book_screen_test, text_correction_editor_test, test/widgets/text)
- ענף: 747 עברו, 4 דולגו, 0 נכשלו. upstream/dev: 691 עברו, 4 דולגו, 0 נכשלו. 56 הבדיקות הנוספות הן החדשות.
- קומיט 1 על dev: 16 נכשלות, 1 עוברת (צירוף בלי הסתרה). כשלים התנהגותיים, לא קומפילציה.
- analyze, format, ui_lint (יציאה 0), generate_plugin_spec --check: נקיים.
- מוטציות: 10 נהרגו; 3 ששרדו אינן חוסמות (ראו BODY).

## חפיפות
- #5683 ו-#5682 כבר מוזגו. ענף fix/correction-editor-no-jump (אין PR) מתנגש בבקר ובפסקה ויצטרך rebase אחרי מיזוג זה.
- #5667 נוגע ב-text_manipulation.dart רק ב-_searchHitStyle; ממוזג נקי.

## החלטות פתוחות
אין. מחוץ להיקף: הדגשת חיפוש. שימוש ב-removeMarks ללא רשימות תווים משלנו.
