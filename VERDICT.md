READY
ענף: fix/correction-editor-search-highlight @ 8cfc97030 (שני קומיטים מעל fix/correction-editor-hide-nikud @ 0b75bc68b, PR #5735). נדחף; PR #5776 פתוח.
כותרת PR: עורך התיקון: הדגשת תוצאות חיפוש כמו בתצוגה

## סבב 6 (rebase)
- נעשה rebase של שני הקומיטים על 0b75bc68b; הקונפליקט היחיד בבקר נפתר בשילוב (שדות ההדגשה לצד `_Edit`/`onEdit`/`_undoValue`, וניתוק המאזין אוחד ל-`dispose` של המתחזק). הקוד של המתחזק לא שונה.
- QA עצמאי: READY. הערה נמוכה שאינה חוסמת: ההחזרה המוקדמת `formats.length != text.length` (תנאי מקורי של הבסיס) לא מוגנת בבדיקה.

## בדיקות
- test/plugins/view + combined_book_screen_test: ענף +536 ~4; בסיס +518 ~4.
- קומיט הבדיקות לבדו על הבסיס: +518 ~4 -18 (כל 18 נכשלות).
- analyze, format, ui_lint (יציאה 0), generate_plugin_spec --check: נקיים, גם על מיזוג זמני עם upstream/dev.
- מוטציות שנתפסות: היפוך merge, הסרת removeListener (ניקוי חיפוש ו-dispose), חישוב מחדש בכל בנייה, התעלמות מ-revision, אי-הודעה על שינוי גרסה, התעלמות מ-limit, קו תחתון ב-IME.
