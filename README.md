# NeCalculator
отиджер приём проверка приём
Связь есть
using System;
using System.Linq;
using System.Windows;
using System.Windows.Controls;

namespace HotelManagment.Pages
{
    public partial class RoomBookingPage : Page
    {
        public RoomBookingPage()
        {
            InitializeComponent();
            UpdateGrid();
            CBoxClient.SelectedIndex = 0;
            CBoxRoom.SelectedIndex = 0;
        }
        private void UpdateGrid()
        {
            var bookings = App.Context.Bookings.ToList();
            DBGrid.ItemsSource = bookings;
        }

        private void CBoxRoom_Loaded(object sender, RoutedEventArgs e)
        {
            var Rooms = App.Context.Rooms.ToList();
            CBoxRoom.DisplayMemberPath = "RoomName";
            CBoxRoom.ItemsSource = Rooms;
        }

        private void CBoxClient_Loaded(object sender, RoutedEventArgs e)
        {
            var Clients = App.Context.Clients.ToList();
            CBoxClient.DisplayMemberPath = "Name";
            CBoxClient.ItemsSource = Clients;
        }

        private void BtnBooking_Click(object sender, RoutedEventArgs e)
        {
            if (TBoxDuration.Text != "" && DPickerDate.SelectedDate != null)
            {
                if (DPickerDate.SelectedDate > DateTime.Today)
                {
                    var RoomsID = App.Context.Rooms.FirstOrDefault(p => p.RoomName == CBoxRoom.SelectedValue.ToString());
                    var ClientID = App.Context.Clients.FirstOrDefault(p => p.Name == CBoxClient.SelectedValue.ToString());
                    if (RoomsID.Status == "Свободна")
                    {
                        var Booking = new Entities.Booking
                        {
                            RoomID = RoomsID.ID,
                            ClientID = ClientID.ID,
                            Date = DPickerDate.SelectedDate.Value,
                            Duration = int.Parse(TBoxDuration.Text),
                            Price = int.Parse(TBlockPrice.Text),
                        };
                        RoomsID.Status = "Забронирована";
                        App.Context.Bookings.Add(Booking);
                        App.Context.SaveChanges();
                        UpdateGrid();
                    }
                    else
                    {
                        MessageBox.Show("Выбранная комната уже забронирована!", "Ошибка", MessageBoxButton.OK, MessageBoxImage.Error);
                    }
                }
                else
                {
                    MessageBox.Show("Вы указали дату меньше сегодняшней!", "Ошибка", MessageBoxButton.OK, MessageBoxImage.Error);
                }
            }
            else
            {
                MessageBox.Show("Все поля должны быть заполнены", "Ошибка", MessageBoxButton.OK, MessageBoxImage.Error);
            }
        }

        private void BtnDelete_Click(object sender, RoutedEventArgs e)
        {
            if (DBGrid.SelectedItems.Count > 0)
            {
                Entities.Booking item = DBGrid.SelectedItem as Entities.Booking;
                var DeleteBooking = App.Context.Bookings.FirstOrDefault(p => p.RoomID == item.RoomID && p.ClientID == item.ClientID);
                var RoomsID = App.Context.Rooms.FirstOrDefault(p => p.ID == DeleteBooking.RoomID);
                if (MessageBox.Show($"Вы уверены, что хотите отменить бронирование?", "Внимание", MessageBoxButton.YesNo, MessageBoxImage.Question) == MessageBoxResult.Yes)
                {
                    App.Context.Bookings.Remove(DeleteBooking);
                    RoomsID.Status = "Свободна";
                    App.Context.SaveChanges();
                    UpdateGrid();
                }
            }
            else
                MessageBox.Show("Вы ничего не выбрали для удаления!", "Внимание", MessageBoxButton.OK, MessageBoxImage.Question);
        }

        private void TBoxDuration_SelectionChanged(object sender, RoutedEventArgs e)
        {
            if (TBoxDuration.Text != "")
            {
                var RoomsID = App.Context.Rooms.FirstOrDefault(p => p.RoomName == CBoxRoom.SelectedValue.ToString());
                var Category = App.Context.Categories.FirstOrDefault(p => p.ID == RoomsID.CategoryID);

                bool checkprice = int.TryParse(TBoxDuration.Text, out var price);
                if (checkprice)
                    TBlockPrice.Text = (int.Parse(TBoxDuration.Text) * Category.CategoryPrice).ToString();
                else
                    MessageBox.Show("Вы указали неверный формат!", "Внимание", MessageBoxButton.OK, MessageBoxImage.Question);
            }
        }
    }
}
