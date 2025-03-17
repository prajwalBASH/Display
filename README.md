# Display
#include <QApplication>
#include <QTableWidget>
#include <QHeaderView>
#include <QDropEvent>

class DraggableTableWidget : public QTableWidget {
public:
    DraggableTableWidget(QWidget *parent = nullptr) : QTableWidget(parent) {
        setColumnCount(3);
        setRowCount(5);
        setHorizontalHeaderLabels({"Column 1", "Column 2", "Column 3"});

        // Fill table with sample data
        for (int row = 0; row < rowCount(); ++row) {
            for (int col = 0; col < columnCount(); ++col) {
                setItem(row, col, new QTableWidgetItem(QString("Item %1-%2").arg(row).arg(col)));
            }
        }

        setSelectionBehavior(QAbstractItemView::SelectRows);
        setDragDropMode(QAbstractItemView::InternalMove);
        setDragEnabled(true);
        setDropIndicatorShown(true);
        setDefaultDropAction(Qt::MoveAction);
    }

protected:
    void dropEvent(QDropEvent *event) override {
        QModelIndex droppedIndex = indexAt(event->position().toPoint());

        if (!droppedIndex.isValid()) {
            event->ignore();
            return;
        }

        int fromRow = currentRow();
        int toRow = droppedIndex.row();

        if (fromRow != toRow) {
            moveRow(fromRow, toRow);
        }
    }

private:
    void moveRow(int fromRow, int toRow) {
        if (fromRow < 0 || toRow < 0 || fromRow == toRow)
            return;

        // Copy the original row data
        QList<QTableWidgetItem*> rowItems;
        for (int col = 0; col < columnCount(); ++col) {
            rowItems.append(new QTableWidgetItem(*item(fromRow, col)));
        }

        // Remove the original row
        removeRow(fromRow);

        // Insert a new row at the target position
        insertRow(toRow);
        for (int col = 0; col < columnCount(); ++col) {
            setItem(toRow, col, rowItems.at(col));
        }

        // Select the newly moved row
        setCurrentCell(toRow, 0);
    }
};

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    DraggableTableWidget table;
    table.show();
    return app.exec();
}
    void updateEuItemsOrder(bool moveUp);
void BarGraphEuDataSelection::updateEuItemsOrder(bool moveUp)
{
    //if (this->ui.pushButtonUp)
    //{
    //    int currentRow = this->ui.barGraphParameterTableWidget->currentRow();
    //    if (currentRow > 0) {
    //        // Move item up
    //        QTableWidgetItem* item = ui.barGraphParameterTableWidget->takeItem(currentRow, 0);
    //        ui.barGraphParameterTableWidget->setItem(currentRow - 1, 0, item);
    //        // Finally keep the item selected
    //        ui.barGraphParameterTableWidget->selectRow(currentRow - 1); 
    //    }
    //}
    //else if(this->ui.pushButtonDown)
    //{
    //    int currentRow = this->ui.barGraphParameterTableWidget->currentRow();
    //    if (currentRow < ui.barGraphParameterTableWidget->rowCount() - 1) {
    //        // Move item down
    //        QTableWidgetItem* item = ui.barGraphParameterTableWidget->takeItem(currentRow, 0);
    //        ui.barGraphParameterTableWidget->setItem(currentRow + 1, 0, item);
    //        // Finally keep the item selected
    //        ui.barGraphParameterTableWidget->selectRow(currentRow + 1);
    //    }
    //}
    // Check if the up button was pressed
    int currentRow = this->ui.barGraphParameterTableWidget->currentRow();
    int WidgetCoulumnsCount = ui.barGraphParameterTableWidget->columnCount();
    int WidgetRowCount = ui.barGraphParameterTableWidget->rowCount();
    QList<QTableWidgetItem*> qlQTableWidgetItem;
        if (moveUp && currentRow > 0) 
        {
            for (int i = 0; i < WidgetCoulumnsCount; ++i)
            {
                // Move item up
                QTableWidgetItem* item = this->ui.barGraphParameterTableWidget->takeItem(currentRow-1, i);
                qlQTableWidgetItem.append(item);
                //ui.barGraphParameterTableWidget->removeRow(currentRow);void BarGraphEuDataSelection::updateEuItemsOrder(

    // Boundary conditions
    if ((moveUp && currentRow <= 0) || (!moveUp && currentRow >= rowCount - 1))
        return;

    int targetRow = moveUp ? currentRow - 1 : currentRow + 1;

    // Swap entire row contents
    for (int col = 0; col < columnCount; ++col) {
        QTableWidgetItem *currentItem = table->takeItem(currentRow, col);
        QTableWidgetItem *targetItem = table->takeItem(targetRow, col);

        table->setItem(currentRow, col, targetItem);
        table->setItem(targetRow, col, currentItem);
    }

    // Select the moved row
    table->selectRow(targetRow);
}



                //QTableWidgetItem* tempItem = item->clone();
                // Move the item to the row above

                this->ui.barGraphParameterTableWidget->setItem(currentRow - 1, i, item);
            }

            
            
            //this->ui.barGraphParameterTableWidget->setItem(currentRow - 1, 0, item);

            // Move the existing item from the row above down to the currentRow
            //QTableWidgetItem* itemAbove = this->ui.barGraphParameterTableWidget->takeItem(currentRow - 1, 0);
            //this->ui.barGraphParameterTableWidget->setItem(currentRow, 0, itemAbove);

            // Select the new current row
            this->ui.barGraphParameterTableWidget->selectRow(currentRow - 1);
        }
        else if (!moveUp && currentRow < this->ui.barGraphParameterTableWidget->rowCount() - 1) {
            // Move item down
            QTableWidgetItem* item = this->ui.barGraphParameterTableWidget->takeItem(currentRow, 0);

            // Move the item to the row below
            this->ui.barGraphParameterTableWidget->setItem(currentRow + 1, 0, item);

            // Move the existing item from the row below up to the currentRow
            QTableWidgetItem* itemBelow = this->ui.barGraphParameterTableWidget->takeItem(currentRow + 1, 0);
            this->ui.barGraphParameterTableWidget->setItem(currentRow, 0, itemBelow);

            // Select the new current row

void BarGraphEuDataSelection::updateEuItemsOrder(bool moveUp) {
    QTableWidget *table = this->ui.barGraphParameterTableWidget;
    int currentRow = table->currentRow();
    int columnCount = table->columnCount();
    int rowCount = table->rowCount();

    // Boundary conditions
    if ((moveUp && currentRow <= 0) || (!moveUp && currentRow >= rowCount - 1))
        return;

    int targetRow = moveUp ? currentRow - 1 : currentRow + 1;

    // Swap entire row contents
    for (int col = 0; col < columnCount; ++col) {
        QTableWidgetItem *currentItem = table->takeItem(currentRow, col);
        QTableWidgetItem *targetItem = table->takeItem(targetRow, col);

        table->setItem(currentRow, col, targetItem);
        table->setItem(targetRow, col, currentItem);
    }

    // Select the moved row
    table->selectRow(targetRow);
}

