<p>
&nbsp;&nbsp;&nbsp;&nbsp; ���������Goд�ĵ�һ�����������ܻ�����ЩBUGû�в��Ե���������Ҫ���ṩһ���ο�����λ���Ը�д���Լ��ķ��<br />
</p>
<p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ����������������������������а�װ</p>
<div style="background-color: F8F8F8">
<pre>
		go get github.com/male110/SimpleDb
		go install github.com/male110/SimpleDb

</pre>
    </div>
    
<p>
    Go���Ե����ݿ������ֻ����Rows.Scan��һ���Զ�ȡ�����С��о��ܲ�ϰ�ߣ��һ���ϰ�߰���������һ���е�ȡ���ݡ��������Լ���װ��һ�����ݽṹMyRows,MyRowsʵ����һ��������<span
        style="color: #000000;">GetValue(/span>name<span style="color: #c0c0c0;"> </span>
    <span style="color: #000080;">string</span><span style="color: #000000;">,</span><span
        style="color: #c0c0c0;"> </span>value<span style="color: #c0c0c0;"> </span><span
            style="font-weight: 600; color: #000080;">interface</span><span style="color: #000000;">{})���԰�������ȡ���ݡ�������ʾ��</span></p>
<div style="background-color: F8F8F8">
<pre>
		err = rows.GetValue("IsBoy", &isBoy)
		if err != nil {
			fmt.Println(err)
			return
		}
</pre>
    </div>
<p>
    &nbsp;&nbsp;&nbsp; Ϊ�˲����������������������Ľṹ�壬��MyDb����Query��������ֱ�ӷ���&nbsp;MyRows��NewDb��������MyDb�ṹ���������sql.Openһ������ô��ȡ��������ʹ�õ���������</p>
<div style="background-color: F8F8F8">
<pre>
    db, err := SimpleDb.NewDb("mysql", "root:123@tcp(127.0.0.1:3306)/test?charset=utf8")
	if err != nil {
		fmt.Println("��SQLʱ����:", err.Error())
		return
	}
	var rows *SimpleDb.MyRows
	//�����ݿ���ȡ����
	rows, err = db.Query("select * from person")
	if err != nil {
		fmt.Println(err)
		return
	}
	//��ʾ����
	for rows.Next() {
		var id, age int
		var name string
		var isBoy bool
		//���ֶ���ȡ����,Ҳ������rows.Scan(&id,&name,&age),��ȡ
		rows.GetValue("id", &id)
		rows.GetValue("name", &name)

		rows.GetValue("age", &age)
		//���Ը��ݷ���ֵ���ж��Ƿ�ɹ�
		err = rows.GetValue("IsBoy", &isBoy)
		if err != nil {
			fmt.Println(err)
			return
		}
		fmt.Println(id, "\t", name, "\t", age, "\t", isBoy)
	}
</pre>
    </div>
<p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    ͬʱ��ʱ����һ���򵥵�ORM��ʵ����������Ĳ������ݣ��޸����ݣ�ɾ�����ݡ� 
    ��һ��ʹ��ORMֻ����ô���������������Ķ���дSQL��䡣����ֻ��һ���ο�����ҿ��Ը����Լ�����Ҫ���Լ�ϰ�ߣ������޸ġ��ĳ��Լ���Ҫ�ĸ�ʽ�����ݽṹ�Ķ����ʽ���£�</p>
<div style="background-color: F8F8F8">
<pre>
type Person struct {
	/*TableName����ֻ���������ñ���������ṹ������������ͬ����ʡ��*/
	TableName SimpleDb.TableName "person"

	/*name�Ǳ���,PK���������Ƿ�������true������false������*/
	Id int `name:"id"PK:"true"Auto:"true"`

	Name   string "name" //tag���name���Ƕ�Ӧ���ֶ���
	Age    int    "age"  //tag���age���Ƕ�Ӧ���ֶ���
	IsBoy  bool
	NotUse string "-" //-���ᱣ�浽���ݿ���
}
</pre>
</div>
<p>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
�����˵���Ѿ�����ϸ�ˣ�
SimpleDb.TableName���͵��ֶΣ�ֻ������tag�ж���ṹ���Ӧ�ı��������û�и��ֶΣ���Ϊ�������ǽṹ������PK:&quot;true&quot;��ʾ��������Auto:&quot;true&quot;��ʾ���ֶ����Զ��������У�name:&quot;id&quot;,��ָ�ָ��ֶζ�Ӧ�����ݱ��е��������粻ָ����Ϊ���ֶ�����ͬ��&quot;name&quot;��&quot;age&quot;����ֻ��Ҫָ������ʱ������ֱ��д��tag�С�tagΪ&quot;-&quot;��ʾ����Ӧ���ݱ��е��κ��С�</p>

<div style="background-color: F8F8F8">
<pre>
// ��������
p := &Person{Name: "������", Age: 500, IsBoy: true}
db.Insert(p)
//�޸�����
db.Update(p)
//ɾ������
db.Delete(p)
</pre>
</div>
<p>
   ��������һ�����������ӣ�����������:</p>
<div style="background-color: F8F8F8">
<pre>
CREATE TABLE `person` (
	`id` INT(50) NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(50) NULL DEFAULT NULL,
	`age` INT(50) NULL DEFAULT NULL,
	`IsBoy` SMALLINT(10) NULL,
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci';

insert into `person` (name,age,IsBoy) values('����',20,0);
insert into `person` (name,age,IsBoy) values('����',19,1);
</pre>
</div>
</span>
<p>
    �����������Ĵ���</p>
<div style="background-color: F8F8F8">
<pre>
package main

import (
	"fmt"
	_ "github.com/go-sql-driver/mysql"
	"github.com/male110/SimpleDb"
)

type Person struct {
	/*TableName����ֻ���������ñ���������ṹ������������ͬ����ʡ��*/
	TableName SimpleDb.TableName "person"

	/*name�Ǳ���,PK���������Ƿ�������true������false������*/
	Id int `name:"id"PK:"true"Auto:"true"`

	Name   string "name" //tag���name���Ƕ�Ӧ���ֶ���
	Age    int    "age"  //tag���age���Ƕ�Ӧ���ֶ���
	IsBoy  bool
	NotUse string "-" //-���ᱣ�浽���ݿ���
}

func main() {
	db, err := SimpleDb.NewDb("mysql", "root:123@tcp(127.0.0.1:3306)/test?charset=utf8")
	if err != nil {
		fmt.Println("��SQLʱ����:", err.Error())
		return
	}
	defer db.Close()
	p := &Person{Name: "������", Age: 500, IsBoy: true}
	//����һ������
	err = db.Insert(p)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("�²������ݵ�ID:", p.Id)
	var rows *SimpleDb.MyRows
	//�����ݿ���ȡ����
	rows, err = db.Query("select * from person")
	if err != nil {
		fmt.Println(err)
		return
	}
	//��ʾ����
	for rows.Next() {
		var id, age int
		var name string
		var isBoy bool
		//���ֶ���ȡ����,Ҳ������rows.Scan(&id,&name,&age),��ȡ
		rows.GetValue("id", &id)
		rows.GetValue("name", &name)

		rows.GetValue("age", &age)
		//���Ը��ݷ���ֵ���ж��Ƿ�ɹ�
		err = rows.GetValue("IsBoy", &isBoy)
		if err != nil {
			fmt.Println(err)
			return
		}
		fmt.Println(id, "\t", name, "\t", age, "\t", isBoy)
	}
	//����ָ���
	fmt.Println("==========���������============")
	p.Name = "����"
	p.Age = 800
	//�޸�����
	_, err = db.Update(p)
	if err != nil {
		fmt.Println(err, "xxxx")
		return
	}
	//QueryDataRows����һ��DataRow���飬DataRow����һmap��������е�����
	var arrRow []SimpleDb.DataRow
	arrRow, err = db.QueryDataRows("select * from person")
	if err != nil {
		fmt.Println(err, "zzzzz")
		return
	}
	for _, row := range arrRow {
		var id, age int
		var name string
		var isBoy bool
		//ֻ�ܰ��ֶ���ȡ����
		row.GetValue("id", &id)
		row.GetValue("name", &name)
		row.GetValue("age", &age)
		//���Ը��ݷ���ֵ���ж��Ƿ�ɹ�
		err = rows.GetValue("IsBoy", &isBoy)
		if err != nil {
			fmt.Println(err)
			return
		}
		fmt.Println(id, "\t", name, "\t", age, isBoy)
	}
	var p2 Person
	p2.Id = p.Id
	//�������������ݿ���ȡ��������
	err = db.Load(&p2)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(p2)
	//��������ɾ��һ������
	db.Delete(p2)
}
</pre>
</div>

