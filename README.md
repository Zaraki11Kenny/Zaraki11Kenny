Sub Process_Globals
    Dim SQL1 As SQL
End Sub

Sub Globals
    Dim txtNom As EditText
    Dim txtPrenom As EditText
    Dim txtAge As EditText
    Dim txtDomaine As EditText
    Dim txtTelephone As EditText
    Dim btnSubmit As Button
End Sub

Sub Activity_Create(FirstTime As Boolean)
    Activity.LoadLayout("Main")
    If FirstTime Then
        SQL1.Initialize(File.DirInternal, "user.db", True)
        SQL1.ExecNonQuery("CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, nom TEXT, prenom TEXT, age INTEGER, domaine TEXT, telephone TEXT)")
    End If
End Sub

Sub btnSubmit_Click
    Dim nom As String = txtNom.Text
    Dim prenom As String = txtPrenom.Text
    Dim age As Int
    Dim domaine As String = txtDomaine.Text
    Dim telephone As String = txtTelephone.Text
    
    ' Validation
    If nom = "" Or prenom = "" Or txtAge.Text = "" Or domaine = "" Or telephone = "" Then
        ToastMessageShow("Veuillez remplir tous les champs", False)
        Return
    End If
    
    ' Convertir l'âge en entier
    Try
        age = txtAge.Text.ToInt
    Catch
        ToastMessageShow("L'âge doit être un nombre", False)
        Return
    End Try
    
    ' Insertion des données
    SQL1.ExecNonQuery2("INSERT INTO users (nom, prenom, age, domaine, telephone) VALUES (?, ?, ?, ?, ?)", Array As Object(nom, prenom, age, domaine, telephone))
    
    ' Redirection vers une autre activité
    StartActivity("DisplayActivity")
End Sub
