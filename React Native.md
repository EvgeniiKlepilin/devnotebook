# React Native
## Scroll View
ScrollViews can be configured to allow paging through views using swiping gestures by using the `pagingEnabled` props. Swiping horizontally between views can also be implemented on Android using the `ViewPager` component.

On iOS a ScrollView with a single item can be used to allow the user to zoom content. Set up the `maximumZoomScale` and `minimumZoomScale` props and your user will be able to use pinch and expand gestures to zoom in and out.

The ScrollView works best to present a small number of things of a limited size. All the elements and views of a `ScrollView` are rendered, even if they are not currently shown on the screen. If you have a long list of items which cannot fit on the screen, you should use a `FlatList` instead.

## Resources to Explore
- https://reactnative.dev/docs/security
- https://reactnative.dev/docs/performance
- https://reactnative.dev/docs/build-speed
- https://reactnative.dev/docs/ram-bundles-inline-requires
- https://reactnative.dev/docs/profiling
- https://reactnative.dev/docs/timers
