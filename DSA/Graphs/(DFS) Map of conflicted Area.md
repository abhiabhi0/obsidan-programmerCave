**1. ‘I’ will take back all land of ‘P’ which is completely surrounded by ‘I’.
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeYyTSfJOg3WEhh6mLBe-mG2U-MrRXZfZJ9hS-iGgAdUpgA61reIu5jZapS8hkccXee4vquwxap9QgUlsEUtktfcUztUSARUgRsvEBrhCseBjc2a2h22mTKPz_5vPhp8Ix_oLW5STKEpX_RiezzNcSrBmI?key=5htWp-2xX_egQvU14bcQKA) 

2. In this we will traverse all the cells which are on the edge of matrix.
    
3. If we encounter ‘P’, then we run dfs on them and make sure all other ’P’ connected to this ‘P’ do not get converted to ‘I’.
    
4. So we will convert all these to something else, for eg Q.
    
5. Then we traverse all cells of matrix and if we encounter any P, convert it to ‘I’
    
6. Then again we traverse all cells and if we encounter any Q, we convert it to ‘P’ 
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeIXKfvTe8SQG6WwVMlKTsetM-QGbOkNRCoZSeKNaHt6qEJTb3uP8mmouVLn_zJO5Zz1sqgnq7DK1mtrGukC_VgIl5heZxvVh_l1z-Ry-D_hMFealcDl8yQSi_cyHHF0BtmJhpIh3vN2q2GyNlgVWgRgOce?key=5htWp-2xX_egQvU14bcQKA)**