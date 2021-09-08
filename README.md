# mysql-trigger

```sql
--Create new trigger
-- https://paiza.io/en/languages/mysql
BEGIN
    DECLARE nb INT;
    DECLARE var_order_code varchar(255);
    SET nb = (SELECT count(*) from order_ads WHERE order_note_ck = new.tranxBody and total_invoice = new.amount);

    IF(nb > 0) THEN
    -- 		set order_code = (SELECT count(order_code) from order_ads WHERE order_note_ck = new.tranxBody and total_invoice = new.amount);
       -- 	INSERT INTO `logs`(`log`) VALUES (order_code);
            SELECT `order_code` INTO var_order_code from order_ads WHERE order_note_ck=new.tranxBody and total_invoice=new.amount; 
            -- INSERT INTO `logs`(`log`) VALUES (new.tranxBody);
            -- INSERT INTO `logs`(`log`) VALUES (new.amount);
            INSERT INTO `logs`(`log`) VALUES (var_order_code);
            UPDATE `request` SET `status` = '1' WHERE `order_code` = var_order_code AND (status=0  OR status=2);       

    ELSE
    	SET nb = 0;
    END IF;
    
END
```
