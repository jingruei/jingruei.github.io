---
layout: post
title:  "Devexpress DockPanel"
date:   2019-11-12 12:04:08 +0800--
categories: [WPF]
tags: [WPF]  
---

# DockerPanel


#### 類似Tab的分頁效果
     - `DocumentPanel`

```xml
                <dxdo:LayoutGroup x:Name="LayoutGroup1" Orientation="Vertical" ItemWidth="3*">
                    <dxdo:LayoutGroup  x:Name="LayoutGroup2" Orientation="Horizontal" ItemHeight="4*">
                        <dxdo:DocumentGroup x:Name="documentGroup1" SelectedTabIndex="0" DestroyOnClosingChildren="False" >
                            <dxdo:DocumentPanel x:Name="dp1" FloatSize="300,250"  Caption="【MAP44】ShipmentOperation"/>
                            <dxdo:DocumentPanel x:Name="dp2" Caption="視窗二" />
                            <dxdo:DocumentGroup.ControlBoxContentTemplate>
                                <DataTemplate>
                                    <Button Content="+"/>
                                </DataTemplate>
                            </dxdo:DocumentGroup.ControlBoxContentTemplate>
                        </dxdo:DocumentGroup>
                    </dxdo:LayoutGroup>
```

#### Loading Window Code Behind
```c#
        void ActivateItem (BaseLayoutItem item)
        {
            dm.LayoutController.Activate(item);
        }

        //Load MyWindow1.xaml into docPanel1
        private void btnSetContent_ItemClick (object sender,DevExpress.Xpf.Bars.ItemClickEventArgs e)
        {
            ActivateItem(dp1);
            dp1.Content = new Uri(@"Ui\ShipmentOperation.xaml",UriKind.Relative);
        }
```
