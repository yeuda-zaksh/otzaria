READY
ענף: fix/correction-editor-keyboard @ 932021f3d (נבדק בריבייס על upstream/dev 3052a7779; שני קומיטים, author+committer = yeuda-zaksh, בלי שום ייחוס AI, בלי שינוי ב-pubspec.lock/pubspec.yaml).
כותרת PR: העורך בתוסף התיקון: Enter מכניס ירידת שורה, חצים נעים בין שורות ופסקאות, Escape יוצא מהעריכה

בדיקות (58 קבצי בדיקה שמייבאים את מה שנגע: rtl_text_field, rtl_hard_break, plugin_correction*, plugin_book_correction_service, plugin_runtime_state):
- ענף: +974, 0 כשלים
- upstream/dev (בלי בדיקות הענף): +956, 0 כשלים
- קומיט 1 (בדיקות בלבד) מעל dev: +960 -14 (אדום, כנדרש). קומיט 1 נוגע בבדיקות בלבד; שינוי הציפייה ב-combined_book_screen_test נמצא בקומיט 2 ומשנה רק את ציפיית ה-\n (דחיית תגיות נשמרת).
- flutter analyze על הקבצים שהשתנו: נקי. dart format: 0 שינויים. ui_lint_cli: יציאה 0, test/design_system/ui_lint_test: +133. generate_plugin_spec --check: נקי.

מוטציות (כל אחת הפילה בדיקה, ואז שוחזרה): שירות הסשן בלי proposed, הדיווח המובנה בלי בדיקת \n, מסירת העמודה בין פסקאות, copyWith של HardBreakValue, הרחבת העלים (_expandSpan), restoredClean, Escape, חיצים בתוך פסקה, TextInputAction.done.
שחזור (b) של המתחזק (Enter בין בַבֹ, מחיקת השנייה, ביטול) נבדק בענף: ניקוד נשמר.

חפיפות: #5551 (plugin_book_correction_service.dart) - git merge-tree נקי. סריקת כל ה-PRs מ-#5540 ומעלה: אין חפיפה נוספת בקבצים.
שינוי ב-QA: ריבייס על upstream/dev (היה 60 קומיטים מאחור), ללא שינוי תוכן. push עם --force-with-lease.
החלטות פתוחות: אין. הערת סגנון קטנה (לא חוסמת): _expandSpan מעתיק רק טקסט וסגנון, ולכן נופלים לציור הרגיל אם בעץ יש משהו שאינו TextSpan.
