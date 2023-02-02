# Загрузить почтовое вложение Gmail 

> **Задача**: Необходимо загрузить вложение (файл Excel) Gmail в заданную Таблицу Google

```js
function saveExcelGmailAttachmentToExistingSheet() {
  const query = ""; // Поисковый запрос, чтобы найти нужное сообщение с вложением
  const targetBookId = ""; // Идентификатор Таблицы, в которую нужно загрузить данные

  const mediaData = GmailApp.search(query)[0]
    .getMessages()[0]
    .getAttachments()[0];

  const resource = {
    title: mediaData.getName(),
    mimetype: mediaData.getContentType(),
    description: "",
  };

  const file = Drive.Files.insert(resource, mediaData, { convert: true });

  const data = SpreadsheetApp.openById(file.id)
    .getSheets()[0]
    .getDataRange()
    .getValues();

  const targetBook = SpreadsheetApp.openById(targetBookId);
  const targetSheet = targetBook.getSheets()[0];
  targetSheet.appendRow([""]);
  targetSheet
    .getRange(targetSheet.getLastRow() + 1, 1, data.length, data[0].length)
    .setValues(data);

  DriveApp.getFileById(file.id).setTrashed(true);
}
```