# CRUD_Web_Internship
CRUD WEB Develop by .NET Framework Web Application 
Imports System.Data.SqlClient
' เชื่อมต่อกับฐานข้อมูล


Public Class WebForm1
    Inherits System.Web.UI.Page
    Dim connectionString As String = ConfigurationManager.ConnectionStrings("NMB_InternshipConnectionString").ConnectionString
    Dim con As New SqlConnection(connectionString)
    Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load

    End Sub

    Protected Sub btn_add_Click(sender As Object, e As EventArgs) Handles btn_add.Click


        con.Open()

            ' สร้างคำสั่ง SQL สำหรับการเพิ่มข้อมูล
            Dim query As String = "INSERT INTO Product_NMB (ProductID, ProductName, Location, Unit, Color) VALUES (@ProductID, @ProductName, @Location, @Unit, @Color)"

            ' สร้าง SqlCommand พร้อมพารามิเตอร์
            Using cmd As New SqlCommand(query, con)
                cmd.Parameters.AddWithValue("@ProductID", txt_productid.Text)
                cmd.Parameters.AddWithValue("@ProductName", txt_productname.Text)
                cmd.Parameters.AddWithValue("@Location", txt_location.Text)
                cmd.Parameters.AddWithValue("@Unit", txt_unit.Text)
                cmd.Parameters.AddWithValue("@Color", txt_color.Text)

                ' ดำเนินการเพิ่มข้อมูล
                cmd.ExecuteNonQuery()
            End Using


        ' รีเฟรช GridView เพื่อแสดงข้อมูลที่เพิ่มเข้าไป
        GridView1.DataBind()
    End Sub

    Protected Sub btn_search_Click(sender As Object, e As EventArgs) Handles btn_search.Click
        ' เชื่อมต่อกับฐานข้อมูล

        con.Open()

            ' สร้างคำสั่ง SQL สำหรับการค้นหา
            Dim query As String = "SELECT * FROM Product_NMB WHERE ProductID = @ProductID"

            ' สร้าง SqlCommand พร้อมพารามิเตอร์
            Using cmd As New SqlCommand(query, con)
                cmd.Parameters.AddWithValue("@ProductID", txt_search.Text)

                ' สร้าง SqlDataReader เพื่อดึงข้อมูลจากฐานข้อมูล
                Using reader As SqlDataReader = cmd.ExecuteReader()
                    If reader.Read() Then
                        ' กำหนดค่าที่ดึงมาจากฐานข้อมูลใน TextBox
                        txt_productid.Text = reader("ProductID").ToString()
                        txt_productname.Text = reader("ProductName").ToString()
                        txt_location.Text = reader("Location").ToString()
                        txt_unit.Text = reader("Unit").ToString()
                        txt_color.Text = reader("Color").ToString()
                    Else
                        ' ถ้าไม่พบข้อมูลในฐานข้อมูล
                        ClearTextBoxes()
                    lbl_message.Text = "ProductID not found."

                End If
                End Using
            End Using

    End Sub


    ' เมทอดสำหรับล้างค่าใน TextBoxes
    Private Sub ClearTextBoxes()
        txt_productid.Text = String.Empty
        txt_productname.Text = String.Empty
        txt_location.Text = String.Empty
        txt_unit.Text = String.Empty
        txt_color.Text = String.Empty
    End Sub

    Protected Sub btn_update_Click(sender As Object, e As EventArgs) Handles btn_update.Click
        con.Open()
        Dim query As String = "UPDATE Product_NMB SET ProductName = @ProductName, Location = @Location, Unit = @Unit, Color = @Color WHERE ProductID = @ProductID"

        ' Create SqlCommand with parameters
        Using cmd As New SqlCommand(query, con)
            cmd.Parameters.AddWithValue("@ProductID", txt_productid.Text)
            cmd.Parameters.AddWithValue("@ProductName", txt_productname.Text)
            cmd.Parameters.AddWithValue("@Location", txt_location.Text)
            cmd.Parameters.AddWithValue("@Unit", txt_unit.Text)
            cmd.Parameters.AddWithValue("@Color", txt_color.Text)

            ' Execute the update command
            cmd.ExecuteNonQuery()
        End Using
        GridView1.DataBind()
    End Sub

    Protected Sub btn_delete_Click(sender As Object, e As EventArgs) Handles btn_delete.Click
        con.Open()
        Dim query As String = "DELETE FROM Product_NMB WHERE ProductID = @ProductID"
        Using cmd As New SqlCommand(query, con)
            cmd.Parameters.AddWithValue("@ProductID", txt_search.Text)
            cmd.ExecuteNonQuery()
        End Using
        GridView1.DataBind()
    End Sub


End Class
