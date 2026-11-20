READY
ענף: fix/correction-editor-no-jump, SHA c95863ec36fda9110b009fa4a1afeb403d0cc18d (שני קומיטים מעל upstream/dev 3052a77: 61f9e13 בדיקות בלבד, c95863e תיקון). author ו-committer: yeuda-zaksh.
כותרת PR: פסקת תיקון: מעבר מקריאה לעורך בלי קפיצה והפעלה בלחיצה אחת

סבב 2 - שורש ההבדל ב-<small>: הוא לא ב-small אלא בעיצוב הסוגריים של הקורא. TextRendererService.processText מפעיל formatTextWithParentheses (RenderSettings.formatParentheses, ברירת מחדל true) שעוטף כל "(...)" ב-<small>. העורך לא עשה זאת. היחס זהה (5/6 = 15/18), ולכן small, big ו-bold בלי סוגריים נתנו 0 אי-התאמות. הסריקה בפועל: 101 רוחבים (180 עד 480 בקפיצות של 3) עם גופן FrankRuehlCLM אמיתי; סוגריים - 10 אי-התאמות לפני התיקון, <small>(…)</small> - 21, <small>(x)</small> - 2, וכולן 0 אחרי. התיקון: PluginCorrectionController מקטין תווים שבין סוגריים ב-kHtmlSmallerFontScale באותו כלל (ב-buildTextSpan, בלי לשנות את מערך העיצובים השמור), והפסקה מעבירה לו settings.formatParentheses.

בדיקות (ריצה על test/plugins/integration/plugin_correction_end_to_end_test.dart, test/plugins/view/plugin_correction_{controller,editing,no_jump,paragraph,session_banner}_test.dart, test/text_book/view/combined_view/combined_book_screen_test.dart):
- ענף: 129 עברו, 0 נכשלו (113 קיימות + 16 חדשות ב-no_jump).
- upstream/dev (בלי קובץ no_jump שלא קיים שם): 113 עברו, 0 נכשלו.
- קומיט 1 בלבד (בדיקות על dev): 113 עברו, 16 נכשלו - כל 16 החדשות נכשלות על dev.
- test/design_system/ui_lint_test.dart: 133 עברו. dart tool/ui_lint_cli.dart: יציאה 0, אין הפרות בקבצי plugin_correction. flutter analyze על lib/plugins/view, test/plugins/view, test/test_helpers: נקי. dart format --set-exit-if-changed על 5 הקבצים: נקי. generate_plugin_spec --check: מעודכן. pubspec.lock לא השתנה.
- שלוש בדיקות גובה-השורות תופסות באמת: הגופן נטען בבדיקה (FontLoader), והשבירה שונה בלי התיקון (נכשלות על dev).

הוכחות מוטציה (כל אחת הפילה בדיקה, והקובץ שוחזר): גודל סוגריים (הסרת _parenthesized), חיווט formatParentheses מהפסקה, נעילת גובה, דיכוי reveal, לחיצה אחת (onTap ריק), יציאה בלחיצה כפולה, _active (הסרת _stopEditing של הקודמת), letterSpacing, wordSpacing, שולי הסמן (_caretMargin).

חפיפות: #5578 ו-#5551 ו-#5643 - git merge-tree מול הענף נקי, ואין בהם קבצי plugin_correction. הערת סיווג: שינויי plugin_correction_editing_test.dart ו-activate_correction_paragraph.dart (התאמת בדיקות קיימות ללחיצה אחת) נמצאים בקומיט 2 ולא בקומיט 1, כי הם חלק מהשינוי ההתנהגותי ועוברים על dev.

החלטות פתוחות: אין. מגבלה מתועדת: כלל הסוגריים בעורך חל על הטקסט הגלוי בלבד, ולכן סוגריים בתוך עוגן קישור פנימי (ש-formatTextWithParentheses מדלג עליו בקורא) יוקטנו בעורך. לא נבדק בבדיקה.
