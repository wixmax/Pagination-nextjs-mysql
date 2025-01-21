
# Pagination باستخدام Cursor في Next.js مع MySQL2 و Axios

## مقدمة
هذا المشروع يوضح كيفية تنفيذ Pagination القائمة على Cursor في تطبيق Next.js باستخدام MySQL2 و Axios. pagination القائمة على Cursor هي تقنية فعالة لتقسيم مجموعات البيانات الكبيرة إلى صفحات أصغر، مما يحسن أداء التطبيق ويجعل تصفح البيانات أكثر سلاسة.

## الميزات الرئيسية
* **Pagination:** تنفيذ Pagination فعال باستخدام Cursor.
* **MySQL:** استخدام MySQL2 للاتصال بقاعدة البيانات.
* **Axios:** استخدام Axios لإجراء طلبات HTTP.
* **Next.js:** بناء الواجهة الأمامية باستخدام Next.js.

## الشروع بالعمل
1. **تثبيت المتطلبات:**
   ```bash
   npm install

## مثال عملي (باستخدام Next.js):

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import mysql from 'mysql2/promise';

const MyComponent = () => {
  const [data, setData] = useState([]);
  const [cursor, setCursor] = useState(null);
  const [isLoading, setIsLoading] = useState(false);
  const [prevCursor, setPrevCursor] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      setIsLoading(true);
      try {
        const connection = await mysql.createConnection({
          // ... معلومات الاتصال بقاعدة البيانات
        });
        const [rows] = await connection.execute(
          'SELECT * FROM your_table WHERE id > ? ORDER BY id ASC LIMIT 10',
          [cursor]
        );
        setData(rows);
        setCursor(rows[rows.length - 1].id);
        setIsLoading(false);
      } catch (error) {
        console.error(error);
        setIsLoading(false);
      }
    };
    fetchData();
  }, [cursor]);

  const handleNextPage = () => {
    setPrevCursor(cursor);
    setCursor(data[data.length - 1].id);
  };

  const handlePrevPage = () => {
    if (prevCursor) {
      setCursor(prevCursor);
      setPrevCursor(null); // مسح قيمة cursor السابقة بعد الاستخدام
    }
  };

  // ... عرض البيانات
};


