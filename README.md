# How to sort TreeView nodes in Xamarin.Forms (SfTreeView)

You can sort the TreeView nodes based on the node properties in Xamarin.Forms [SfTreeView](https://help.syncfusion.com/xamarin/treeview/overview?).

You can also refer th following article.

https://www.syncfusion.com/kb/11315/how-to-sort-treeview-nodes-in-xamarin-forms-sftreeview

**XAML**

ViewModel command bound to sort the nodes of the TreeView.

``` xml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TreeViewXamarin"
             xmlns:syncfusion="clr-namespace:Syncfusion.XForms.TreeView;assembly=Syncfusion.SfTreeView.XForms"
             x:Class="TreeViewXamarin.MainPage">
 
    <ContentPage.BindingContext>
        <local:FileManagerViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>
 
    <StackLayout>
        <Button Text="Sort TreeView" Command="{Binding TreeViewSortCommand}" HeightRequest="50"/>
        <syncfusion:SfTreeView x:Name="treeView" ChildPropertyName="SubFiles" ItemTemplateContextType="Node" AutoExpandMode="AllNodesExpanded" ItemsSource="{Binding ImageNodeInfo}">
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <Grid x:Name="grid" RowSpacing="0" >
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="40" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding Content.ImageIcon}" VerticalOptions="Center" HorizontalOptions="Center" HeightRequest="35" WidthRequest="35"/>
                        <Grid Grid.Column="1" RowSpacing="1" Padding="1,0,0,0" VerticalOptions="Center">
                            <Label LineBreakMode="NoWrap" Text="{Binding Content.ItemName}" VerticalTextAlignment="Center"/>
                        </Grid>
                    </Grid>
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>
        </syncfusion:SfTreeView>
    </StackLayout>
</ContentPage>
```

**C#**

To sort the collection, create a new collection and sort the ViewModel collection based on the model property using [OrderBy](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.orderby?view=netframework-4.8) linq method. Assign the new collection to the ViewModel collection in the command execute method.

``` c#
namespace TreeViewXamarin
{
    public class FileManagerViewModel : INotifyPropertyChanged
    {
        public Command TreeViewSortCommand { get; set; }
 
        public FileManagerViewModel()
        {
            TreeViewSortCommand = new Command(OnTreeViewSortClicked);
        }
        
        private void OnTreeViewSortClicked(object obj)
        {
            this.ImageNodeInfo = new ObservableCollection<FileManager>(ImageNodeInfo.OrderBy(i => i.ItemName));
        }
         }
}
```

**C#**

To notify the collection changed, implement [INotifyPropertyChanged](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.inotifypropertychanged?view=netframework-4.8) for ViewModel and raise [PropertyChanged](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.inotifypropertychanged.propertychanged?view=netframework-4.8) event for collection.

``` c#
namespace TreeViewXamarin
{
    public class FileManagerViewModel : INotifyPropertyChanged
    {
        public ObservableCollection<FileManager> ImageNodeInfo
        {
            get
            {
                return imageNodeInfo;
            }
            set
            {
                this.imageNodeInfo = value;
                this.RaisedOnPropertyChanged("ImageNodeInfo");
            }
        }
 
        public event PropertyChangedEventHandler PropertyChanged;
        public void RaisedOnPropertyChanged(string _PropertyName)
        {
            if (PropertyChanged != null)
            {
                PropertyChanged(this, new PropertyChangedEventArgs(_PropertyName));
            }
        }
    }
}
```
