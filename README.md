# Application-Using-Main-Window
## Introduction 

  **QMainWindow** is designed around common needs for a main window to have.<br />
it has its own layout to which you can add QToolBars, QDockWidgets, a QMenuBar, and a QStatusBar.<br />
The layout has a center area that can be occupied by any kind of widget(QTablewidget,QTextEdit,..).
          
## Objectives of this homwork
<ul>
  <li>Being able to build an application’s entire user interface, complete with menus, toolbars and status bar.</li>
  <li> Being able to Create a MainWindow based application using the designer.</li>
 <li> Being able to code the functionalities of an application.</li>
</ul>

***
## SpreadSheet

in this exercise we will try to built a SpreadSheet application 
that has a QTableWidget in its central widget.
### Creating central widget, menus , toolbars and statusbars:
<ol>
 <li>Create QtableWidget and set as a central widget </li>
 
 ```cpp
 
     Spreadsheet=new QTableWidget;
    Spreadsheet->setColumnCount(20);
    Spreadsheet->setRowCount(20);
    setCentralWidget(Spreadsheet);
 ```
 
  <li>create actions and add to them icons and shortcuts</li>
 
  ```cpp
 
   //action open file
   QPixmap openIcon(":/open.png");
   openFile = new QAction(openIcon,"Open",this);
   openFile->setShortcut(tr("Ctrl+N"));
  ```
 <li>create menus for group actions with method addaction() </li>
 
   ```cpp
 
  QMenu *menuName;
  menuBar()->addMenu("name")
  ```
  <li>create Toolbar(a toolbar provides a movable panel that contains a set of controls) and add actions to it with method addaction()</li>
 
   ```cpp
    auto tool1=addToolBar("File");
    tool1->addAction(NewFile);
    tool1->addAction(SaveFile);
    tool1->addSeparator();
    tool1->addAction(quit);
   ```
  <li>Create statusbars which indicate cellLocation and it change when the item change </li>
 
  ```cpp
   void SpreadSheet::updateStatusBar(int row, int col)
   {
    QString cell{"(%0, %1)"};
   cellLocation->setText(cell.arg(row+1).arg(col+1));

    }
  ```
 
</ol>
when we will complete the creation of all actions and group them in menus we will show a spreadsheet like this :

   ![image](https://user-images.githubusercontent.com/93142901/146652138-14aea14f-27a5-4149-bc12-fb7138801851.png)
   
   ### Functionalities of the SpreadSheet:
 
     <ol>
 
    <ul>Find</ul>
    <ul>Save</ul>
    <lu>Load</ul>
  
   </ol>
   
 ***
 ## Text Editor
  In this exercise we will use the Designer for a fast application creation,and we will set QTextEdit as a central widget.
  ![image](https://user-images.githubusercontent.com/93142901/146657881-8e08725d-1376-4415-8871-9491e097d56f.png)
  
  
 ### Functionalities of the Text Editor:
   #### Functionality of New action
 
 when we click on the action New ,we send triggered() signal,and the slot of this signal is a function that  clear  the currentfile that was originally created with our application,and also clear the text edit widget.
   
 ```cpp
     void MainWindow::on_actionNew_triggered()
    {
    currentfile.clear();
    ui->textEdit->setText(QString());
     }
  ```
 
   #### Functionality of Open action
   
For open a file ,Firstly, we will open a file dialogue to allow user to select which text file,he want to open<br />
Then,we will create a QFile as an object for reading and writting files,and we will store this file inside a currentfile <br />
And Then,if the file cannot open or we have a probleme the user will show a message of warning,else we will set the filename as a title of the  window and creating Qtextstream as an interface for reading text and copy the text in the string using method readAll() in QTextStream
 
 
    
  ```cpp
    void MainWindow::on_actionOpen_triggered()
    {
    QString filename = QFileDialog::getOpenFileName(this,"open the file");
    QFile file(filename);
    currentfile=filename;
    if(!file.open(QIODevice::ReadOnly | QFile::Text)){
        QMessageBox:: warning(this,"warning","cannot open file :"+ file.errorString());
         return;
    }
    setWindowTitle(filename);
    QTextStream in(&file);
    QString text=in.readAll();
    ui->textEdit->setText(text);
    file.close();
   }
  ```
  if we clicked on open action and choose filename program we will show the content of this file:
   ![image](https://user-images.githubusercontent.com/93142901/146659558-c93c30e3-45e6-4b87-a626-fe1ff0b4b552.png)

#### Functionality of Saveas action:
for Saving a file ,Firstly,we will open a file dialogue to allow user to choose when he want save his file,Then,if the file cannot be saved or we have a probleme the user will show a message of warning,else we will create an interface for writing our text stream and copy the text from the text widget and convert it to plain text and then,output to the file .
```cpp
 void MainWindow::on_actionsave_as_triggered()
{
    QString filename=QFileDialog::getSaveFileName(this,"save as");
    QFile file(filename);
    if(!file.open(QFile::WriteOnly | QFile::Text)){
        QMessageBox::warning(this,"Warning","cannot open the file :"+ file.errorString());
        return;
    }
     currentfile=filename;
     setWindowTitle(filename);
     QTextStream out(&file);
     QString text=ui->textEdit->toPlainText();
     out<<text;
     file.close();
}
```
### Functionality of copy action:
the classe QTextEDIT provides a method called copy().
```cpp
 void MainWindow::on_actioncopy_triggered()
{
    ui->textEdit->copy();
}
```
### Functionality  of cut action:
the classe QTextEDIT provides a method called cut().
```cpp
 void MainWindow::on_actioncut_triggered()
{
        ui->textEdit->cut();
}
```
### Functionality of paste action:
the classe QTextEDIT provides a method called paste().
```cpp
 void MainWindow::on_actionpaste_triggered()
{
        ui->textEdit->paste();
}

```
### Functionality of aboutQT action:
we will use a message box for displaying aboutQT.
```cpp
 void MainWindow::on_actionAbout_QT_triggered()
{
      QMessageBox ::aboutQt(this,"about QT");
}
```
## Conclusion
  


  


   


      
 
