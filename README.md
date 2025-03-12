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
