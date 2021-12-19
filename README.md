# Application-Using-Main-Window
## Introduction 

  **QMainWindow** is designed around common needs for a main window to have.<br />
it has its own layout to which you can add QToolBars, QDockWidgets, a QMenuBar, and a QStatusBar.<br />
The layout has a center area that can be occupied by any kind of widget(QTablewidget,QTextEdit,..).
                       ![image](https://user-images.githubusercontent.com/93142901/146675358-bd70f5d8-722c-41f5-8790-010eeac651f7.png)

          
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
  <li>Create statusbars which indicate cellLocation and it change when the item change, that why we make a connection between updateStatu Bar and cellLocation</li>
 
  ```cpp
   void SpreadSheet::updateStatusBar(int row, int col)
   {
    QString cell{"(%0, %1)"};
   cellLocation->setText(cell.arg(row+1).arg(col+1));

    }
  ```
  ```cpp
    //connectting the chane of any element in the spreadsheet with the update status bar
     connect(Spreadsheet,&QTableWidget::cellClicked, this,&spreadsheet::updatestatutbar);
  ```
 
</ol>
when we will complete the creation of all actions and group them in menus we will show a spreadsheet like this :

   ![image](https://user-images.githubusercontent.com/93142901/146652138-14aea14f-27a5-4149-bc12-fb7138801851.png)
   
   ### Functionalities of the SpreadSheet:
 #### Functionality of New File
first step,we create a slot called NewSlot() that clear the Spreadsheet when we click on NewFile action :
  ```cpp
   void spreadsheet::NewSlot(){
    Spreadsheet->clear();}
  ```
  Then,we make a connection between NewFile and NewSlot in the methode makeconnections():
  ```cpp
    //connexion for New File
    connect(NewFile,&QAction::triggered,this,&spreadsheet::NewSlot);
  ```
#### Functionality of AboutQT
first step,we create a slot called InfoAbout() that show a message Box about QT when we click on NewFile action 
 ```cpp
    void spreadsheet::infoAbout(){
     QMessageBox ::aboutQt(this,"about QT");
   }
 ```
Then,we make a connection between NewFile and NewSlot in the methode makeconnections():
  ```cpp
   connect(About,&QAction::triggered,this,&spreadsheet::infoAbout);
  ```
#### Functionality of goCell
when,we click on goCell action we will show a QDialog that has a QLineEdit that allow to user to write which cell he want to go.<br />
and for do that ,the first step is create a Qdialog with class form and then  get the expression which the user write and compare with our regexpression,if it true the user will go to the cell ,else he must try to write expression equal our regexpression.
 ```cpp
   {
     //Creating the dialog
     GoDialog D;
      //Executing the dialog and storing the user response
     auto reply = D.exec();
      //Checking if the dialog is accepted
     
     if(reply == GoDialog::Accepted)
     { //Getting the cell text
         auto cell = D.gocell();
           //letter distance
         int row = cell[0].toLatin1() - 'A';
         cell.remove(0,1);
          //second coordinate
         int col =  cell.toInt();
          //changing the current cell
         Spreadsheet->setCurrentCell(row, col-1);
     }
   }
 ```
 as a result if we click on gocell we will show the following QDialog:<br />
  ![image](https://user-images.githubusercontent.com/93142901/146675854-6c83d596-144c-4b2b-9a69-190b7913d237.png)<br />
 We write A as Row and 15 as a column ,we will go to cell ( A,15)<br />
  ![image](https://user-images.githubusercontent.com/93142901/146675934-5ac74e5b-614c-4d33-b54c-24750654bdd6.png)

#### Functionality of Find:
when,we click on Find action we will show a QDialog that has a QLineEdit that allow to user to write which value he want to find.<br />
and for do that ,the first step is create a Qdialog with class form and then  get the expression which the user write and compare with our regexpression,if it true the user will go to the cell that has the value which the user search,else he must try to write expression equal our regexpression,or the value  not found in any cell.
```cpp
   Dialog D;
    auto replay=D.exec();
    if(replay== QDialog::Accepted){

        auto pattern =D.findcell();

        for(int i=0;i<Spreadsheet->rowCount();i++)
            for(int j=0;j<Spreadsheet->colorCount();j++){
                auto content = Spreadsheet->item(i,j);

              }
    }
```
 as a result if we click on find we will show the following QDialog:<br />
 ![image](https://user-images.githubusercontent.com/93142901/146676279-7dd889f7-aad8-4803-bb01-ab561d761e4f.png)<br />
 we search the cell that containt the value=Web<br />
 ![image](https://user-images.githubusercontent.com/93142901/146676361-95201f6f-bcb9-4223-92da-82e9877a5d2b.png)
 #### Functionality of Save File:
 for Saving a file ,Firstly,we will use a  save content function   to save the content of our spreadsheet in a text file.
 ```cpp
    {

     //Gettign a pointer on the file
     QFile file(filename);

     //Openign the file
     if(file.open(QIODevice::WriteOnly))  //Opening the file in writing mode
     {
         //Initiating a stream using the file
         QTextStream out(&file);

         //loop to save all the content
         for(int i=0; i < Spreadsheet->rowCount();i++)
             for(int j=0; j < Spreadsheet->columnCount(); j++)
             {
                 auto cell = Spreadsheet->item(i, j);

                 //Cecking if the cell is non empty
                 if(cell)
                 out << cell->row() << ", "<< cell->column() << ", " << cell->text() << endl;
             }

     }
     file.close();
    }
 ```
 Then we create a saveSlot and makeconnexion between it and save action
 ```cpp
     QString currentFile=nullptr;

   //Creating a file dialog to choose a file graphically
   auto dialog = new QFileDialog(this);

   //Check if the current file has a name or not
   if(currentFile == "")
   {
      currentFile = dialog->getSaveFileName(this,"choose your file");

      //Update the window title with the file name
      setWindowTitle(currentFile);
   }

  //If we have a name simply save the content
  if( currentFile != "")
  {
          saveContent(currentFile);
   }
 ```
#### Functionality of Load action
For open a file ,Firstly, we will open a file dialogue to allow user to select which text file,he want to open<br />
Then,we will create a QFile as an object for reading and writting files,and we will store this file inside a currentfile <br />
And Then,if the file cannot open or we have a probleme the user will show a message of warning,else we will set the filename as a title of the  window and creating Qtextstream as an interface for reading text and copy the text in the string using method readline() in QTextStream,and store the txt split with , in QlisteString ,so in index 0 in this list we have the row   that we should invert to int and in seconde index we have the column and in th third we have the value ,and for set that in our spreadsheet we use methode setIten(int row,int column,item).
 ```cpp
      Spreadsheet->clear();
     QString filename = QFileDialog::getOpenFileName(this,"open the file");
    QFile file(filename);
    currentfile=filename;
    if(!file.open(QIODevice::ReadOnly | QFile::Text)){
        QMessageBox:: warning(this,"warning","cannot open file :"+ file.errorString());
         return;
    }
    setWindowTitle(filename);
    QTextStream in(&file);

    while(!in.atEnd()){
    QString text=in.readLine();
    QStringList columns=text.split(",");
    for(int j=0;j<columns.size();j++){
      QTableWidgetItem *item= new QTableWidgetItem(columns[2]);
      int k=columns[0].toInt();
      int b=columns[1].toInt();
      Spreadsheet->setItem(k,b,item);
      }
    file.close();

}
 ```
for example if we want load the file "filiere " we will show the result:

![image](https://user-images.githubusercontent.com/93142901/146677382-e2354b22-a5e6-47e4-8eb0-c3437f73407d.png)

 
   
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
  


  


   


      
 
